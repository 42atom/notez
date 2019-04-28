# Node install



#### NVM

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

