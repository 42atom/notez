## Node install



### NVM

* 如果没有安装Homebrew: 

0. 访问 https://brew.sh/index_zh-cn , 安装Homebrew:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

1. `brew install wget`

* 如果没有安装xcode command tools:


2. 安装Xcode  `xcode-select --install`

安装NVM: 

3. 访问并安装nvm https://github.com/creationix/nvm;
如果安装失败可能是缺少“/Users/paulwan/.bash_profile”文件.

4. 输入  `nvm --version`  查询版本

5. 输入 `nvm install stable` 安装node










N

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

Tips:  有时候centos会出现使用sudo找不到命令-- command not found, 建立一软下链接即可:

```
sudo ln -s /usr/local/bin/npm /usr/bin/npm
sudo ln -s /usr/local/bin/node /usr/bin/node
sudo ln -s /usr/local/bin/n /usr/bin/n
```

n 介绍:

n是Node的一个模块，作者是TJ, Holowaychuk（鼎鼎大名的Express框架作者），就像它的名字一样，它的理念就是简单：

"no subshells, no profile setup, no convoluted api, just simple"

安装很简单：

$ sudo npm install -g n 安装完成之后，直接输入n后输出当前已经安装的node版本以及正在使用的版本（前面有一个o），你可以通过移动上下方向键来选择要使用的版本，最后按回车生效。

```
$ n
    0.10.1
    0.10.15
o   0.10.21
    0.11.8
```

如果你要安装其他的版本（比如0.11.12），那么如下：

```
$ n 0.11.12
install : 0.11.12
   mkdir : /usr/local/n/versions/0.11.12
   fetch : http://nodejs.org/dist/v0.11.12/node-v0.11.12-darwin-x64.tar.gz
####                                                     5.9%
```

安装最新的版本

`$ n latest`

安装稳定版本

`$ n stable`

删除某个版本

`$ n rm 0.10.1`

以指定的版本来执行脚本

`$ n use 0.10.21 some.js`

如果没有将n安装到指定目录,会出现node版本还是和以前一样, 参考[http://www.jb51.net/article/98153.htm](http://www.jb51.net/article/98153.htm)



# 关于异步/同步 读写文件等：

  
在Node.js中读写大文件 [https://cnodejs.org/topic/55a73038f73c01466cf931f2](https://cnodejs.org/topic/55a73038f73c01466cf931f2)

《单线程的 Node.js》 http://blog.csdn.net/xjtroddy/article/details/51388655


# Mac OS NPM等 代理设置

有时候因为GFW，SSR设置了全局依旧是不行……因为终端是独立的。在图形界面设置的代理并不会影响终端的工作。\(包括git,brew,npm等类似的命令行程序\)。

解决方法：

我们得知Shadowsocks-NG可以提供HTTP和HTTPS代理了,那么我们可以单独为某些命令配置代理.

> 参考[https://docs.npmjs.com/files/npmrc](https://docs.npmjs.com/files/npmrc)

首先，编辑npmrc，使用终端：

```
vim ~/.npmrc
```

然后，加入指定代理和端口，这是根据Shadowsocks-NG的【设置】指定的。

```
proxy = http://127.0.0.1:1087
https-proxy = http://127.0.0.1:1087
```

然后保存退出即可，如果想使用命令直接配置，参考文章

[命令配置npmrc](http://www.cnblogs.com/huang0925/archive/2013/05/17/3083207.html)

对配置文件进行更详细的解释

---

如果其它如brew、git等也遇到类似问题：

临时的方法：

```
export http_proxy=http://ipAddress:portNumber;
export https_proxy=http://ipAddress:portNumber;
```

永久的方法：把以上设置写到 ~/.bash\_profile file 中

```
vim ~/.bash_profile
```

这样SSR就会发挥它的作用了。



### NodeJs VPS配置

Express源码阅读 [https://github.com/sunkuo/grow-to-express](https://github.com/sunkuo/grow-to-express)

配置

[参考1](http://www.jb51.net/article/55564.htm)  ;  

[参考2](http://blog.csdn.net/kaosini/article/details/50471960) ; 

[参考3](https://www.v2ex.com/t/387783)

```
配置方法： 
1）在配置文件http段内容添加后端服务器： 
http {     #添加后端服务器，和nginx负载均衡配置一样     
    upstream nodejs {         
        server 127.0.0.1:8080;     
    }     
    ...
} 

2）给应用添加虚拟主机： 
server {     
    listen       80;
    server_name  node.xxxx.com;
    location / {
             proxy_pass http://nodejs;#名字和前面的对应，将所有的请求转发给后端的node
     }     
     access_log  logs/nodejs.access.log  main;#如果需要日志的话
 } 

推荐将静态文件如css、js和图片和应用服务器分开。 
应用启动的话可以直接node app.js，还可以使用其他守护方式启动，当进程挂了自动重启。
还可以考虑使用multi-node（https://github.com/kriszyp/multi-node），增强应用的稳定性和性能。
```



