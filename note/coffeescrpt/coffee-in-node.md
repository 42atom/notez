# 在node环境下使用coffee



方法1:  coffeescript 2 参考 [Coffee官方文档](http://coffeescript.org/#nodejs-usage) 

coffeeScript 1.0 的node  [参考文档](http://www.marcusoft.net/2015/03/node-with-coffeescript-not-a-piece-of-cake.html) 

node模块的coffeescript[ 参考文档](http://nickdesaulniers.github.io/blog/2013/08/28/making-great-node-dot-js-modules-with-coffeescript/)



方法2: 最简单的用法:

1.`npm install coffeescript`

2. 在package.json里的script 加上`  "start": "coffee index.coffee --nodejs"`, 意味着直接用coffee运行,不用管js;

或者, 使用bash运行:  `coffee -w -c *`**`.coffee` , 然后在package.json里的script 里指定使用编译后的**`***.js`.

coffee2的编译注意可以是ES6.

##### coffeescript 1 和 2 的区别

##### 请参考: http://coffeescript.org/\#breaking-changes



