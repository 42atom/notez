# Linux About


[参考 DO 服务器初始化配置](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7)

##### 用户账号配置

安装完之后,即可修改root密码, 修改root 密码  
`$ sudo passwd root`

`sudo -i` # 切换root, exit 退回

##### 配置用户
新增普通用户paulwan,并加入wheel组
``` bash
adduser paulwan
passwd paulwan
gpasswd -a paulwan wheel
```

##### 赋予root权限

修改 `vi /etc/sudoers` 文件，找到下面一行， 在root下面添加一行，如下所示：

```
Allow root to run any commands anywhere
root ALL=(ALL) ALL paul ALL=(ALL) ALL
```

修改完毕，现在可以用paul帐号登录，然后用命令 su - ，即可获得root权限进行操作。

##### 安装VIM
`yum install vim-enhanced`

##### SSH与防火墙配置

`sudo vi /etc/ssh/sshd_config`

优化速度和稳定

```
PermitRootLogin no # 将 `PermitRootLogin yes` 修改,关闭root远程登录
Protocol 2 # 更改协议版本为2,添加
Port 36789 # 修改端口,寻找：`＃Port 22` 修改

PermitEmptyPasswords no # 禁用空白密码,打开

UseDNS=no #关闭dns解析,修改

GSSAPIAuthentication no  #关闭GSS认证

ClientAliveInterval 60 #调整ssh链接不断开的时间
ClientAliveCountMax 3 #调整ssh链接数
```
跟着重新启动 sshd：

`systemctl restart sshd.service`


安装防火墙 

`yum install net-tools`

打开防火墙等端口管理,并使用36789做ssh

```
systemctl start firewalld
systemctl enable firewalld
# 防火墙添加端口(或开放80等)
firewall-cmd --zone=public --add-port=36789/tcp --permanent
# 校验端口是否开放
firewall-cmd --query-port=36789/tcp
# 重启 firewall
sudo firewall-cmd --reload
# 列出所有开放端口
firewall-cmd --list-all
```

centos7 防火墙参考[相关设置看这里](http://blog.csdn.net/MikeLC7/article/details/73549515)

##### Git 安装

```
sudo yum install git
git --version  # 检查版本
git config --global user.name "YourName"  # 设定用户
git config --global user.email "**@***.com"  # 设定邮件
```

确认关闭selinux `sudo sestatus` ,SELinux status:应该是disabled

[参看内容](https://www.brilliantcode.net/145/centos-7-check-selinux-status-enabled-or-not/)

##### 开启SFTP

端口一般和SSH一样

```
sudo groupadd sftp_group   # 為設定存取權限，新增使用者群組

sudo useradd sftpuser -g sftp_group -s /sbin/nologin   # 新增sftpuser用户，并只能使用SFTP且禁止登入伺服器

sudo passwd sftpuser   # 替使用者設定密碼

sudo mkdir /home/sftpuser/data    # 建立其目录
```
设定文件权限

```
sudo chown root /home/sftpuser/
sudo chmod 755 /home/sftpuser/
sudo chown root /home/sftpuser/
sudo chown sftpuser. /home/sftpuser/data/
sudo chmod 755 /home/sftpuser/data
```

检查：查看文件夹权限，应如下否则访问权限会拒绝。
```
cd /home
ls -al
```
```
drwxr-xr-x.  4 root     root      37 Mar 19 20:53 .
dr-xr-xr-x. 17 root     root       244 Jan 13 18:58 ..
drwxr-xr-x   4 paulwan  paulwan    129 Mar 19 20:56 paulwan
drwxr-xr-x   3 root sftp_group  74 Mar 19 21:15 sftpuser
```

```
cd sftpuser
ls -l
```
```
drwxr-xr-x 2 sftpuser sftp_group 6 Mar 19 21:15 data
```

[参看](https://www.brilliantcode.net/142/centos-7-configure-sftp/)内容


##### SSR 安装 in CentOS

SSR 详细安装说明 [参考这里](https://teddysun.com/339.html),.     
---- 注意没有设置为开机启动,系统重启后需手动打开

```
yum install python-setuptools && easy_install pip
pip install shadowsocks
```
##### 开始SSR后台服务
```
sudo ssserver -p 443 -k pass***d -m aes-256-cfb --user nobody -d start
# 停止 SSR
sudo ssserver -d stop
# 校验端口
firewall-cmd --query-port=443/tcp
# 开放端口
sudo firewall-cmd --zone=public --add-port=443/tcp --permanent
```

#### nginx 安装

##### 安装 nginx
参考[CentOS配置 Nginx](http://www.linuxidc.com/Linux/2016-09/134907.htm), 
以[su]使用root身份;

编译依赖 gcc 环境
`yum install gcc-c++`

PCRE pcre-devel 安装; zlib 安装; OpenSSL 安装
```
yum install -y pcre pcre-devel zlib zlib-devel openssl openssl-devel
```

下载 https://nginx.org/en/download.html 最稳定版
```
yum install wget
wget -c https://nginx.org/download/nginx-1.12.2.tar.gz
tar -zxvf nginx-1.12.2.tar.gz
cd nginx-1.12.2
./configure
make
make install
```
查看安装路径
`whereis nginx`

默认安装位置 `/usr/local/nginx`，可以`cd /usr/local/nginx/sbin/`

常用命令
```
./nginx  # 启动
./nginx -s stop  # 停止
./nginx -s quit  # 退出
./nginx -s reload  # 重启
```
查询nginx进程
`ps aux|grep nginx`

开机启动
`vi /etc/rc.local`
增加一行 `/usr/local/nginx/sbin/nginx`

设置执行权限
`chmod 755 /etc/rc.local`


centos 里编辑conf 文件
```
sudo vim /usr/local/nginx/conf/nginx.conf # 编辑conf文件
cd /usr/local/nginx/sbin/
sudo ./nginx -s reload
```
 [nginx服务器绑定域名和设置根目录的方法](http://www.cnblogs.com/freeweb/p/5261077.html)

如果出现403错误，需要网站目录给予755权限（含父目录） 

- 或 SELinux问题：\`setenforce 0\`, 参考
[这里](https://stackoverflow.com/questions/22586166/why-does-nginx-return-a-403-even-though-all-permissions-are-set-properly%29) 

- error\_log位置\` /var/log/nginx/error.log\`

参考：[来点 Nginx](https://www.v2ex.com/t/387783)

`sudo gpasswd -a nginx wheel` 
`sudo nginx -s reload`


##### 配置nginx


保存文件后，输入sudo nginx -t测试我们的配置文件是否有错误，一般错误都是漏个分号，少个字母之类的，错误提示很精确，没错的话会输出下面两句:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

现在我们需要重启Nginx，我们的配置文件才会生效;

如果遇见配置的其他虚拟服务器有问题，可以这样排查：

1.防火墙是否未开放端口；
2.nginx配置文件正确吗；
3.相应的根目录是否有文件。


##### 修改nginx配置
首先进入nginx安装目录，然后执行 vim conf/nginx.conf 打开nginx的配置文件:

sudo vim /usr/local/nginx/conf/nginx.conf
cd /usr/local/nginx/sbin/
sudo ./nginx -s reload


##### Node & N 安装

```
git clone https://github.com/tj/n
cd n
sudo make install
```

安装n之后再安装node.

```
$ n latest //最新
$ n stable  // 稳定最新
$ n lts  // 长期支持
```


Tips:  有时候centos会出现使用sudo找不到命令-- command not found, 建立一下软链接即可:

```
sudo ln -s /usr/local/bin/npm /usr/bin/npm
sudo ln -s /usr/local/bin/node /usr/bin/node
sudo ln -s /usr/local/bin/n /usr/bin/n
```


##### mac 上远程挂载

1. 安装sshfs： \`brew install sshfs\`

2. Install \[osxfuse\]\([https://osxfuse.github.io\](https://osxfuse.github.io%29\)

3. 创建remote用的文件夹

