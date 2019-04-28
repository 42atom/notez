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

