coffee snippet

##### 阶乘写法，非尾递归

```coffeescript
f = (n) ->
  if n is 1
    1
  else
    n * f(n - 1)

console.log '阶乘 = ' + result = f(6)
console.log result = 1 * 2 * 3 * 4 * 5 * 6
```
##### 汉诺塔样例
```
step = 0
hanoi = (disc, src, aux, dst) ->
    if disc > 0
        hanoi(disc - 1, src, dst, aux) #挪动上面的圆盘到辅助柱上
        step += 1
        console.log(" Move disc", disc, "from", src, "to", dst) #挪动下面的圆盘到目标柱上
        hanoi(disc - 1, aux, src, dst) #挪动辅助柱上的圆盘到目标柱上
    else console.log '汉诺塔步数' + step

# hanoi(3, "Src", "Aux", "Dst")


# 多参数样例
multiPram = (first, second, others...) ->
 console.log [first, second, others, arguments[2]]

multiPram(1, 2, 3, 4)

# 打印信息类型
consoleSample = ->
  console.warn "警告信息"
  console.error "错误信息"
  console.info "提示信息"

  # 打印分组信息
  console.group '第1组'
  console.log '1-1'
  console.log '1-2'
  console.groupEnd()

  console.group '第2组'
  console.log '2-1'

  # 打印运行时间，包裹执行语句后输出
  console.time('运行时间是：')
  for i in [0..10000]
    tableObj = i
  console.timeEnd('运行时间是：')
# consoleSample()

# 如何打印断言，参数1为真时，不会抛出错误，否则抛出错误
assertFunc = ->
  return 10

console.assert( assertFunc() is 10, '这里出现问题！')


# 链式语法
linkStyle = {
  method1: () ->
    console.log 'method a'
    return @
  method2: () ->
    console.log 'method b'
    return @
  method3: () ->
    console.log 'method c'
    return @
}
# 调用
linkStyle.method1().method2().method3()


# 计数异步执行, await 的使用
# 异步调用主要解决js里同步执行与回调层次过多的问题，依赖于promise化函数，使用await调用（阻塞执行）；
# await 是个运算符，取决于等到的东西，如果是promise对象，就会阻塞后面的执行（直到该对象resolve，获取resolved的值），
# 如果等待到的不是promise对象，那么运算结果就是它等到的东西。函数前面的async一词意味着一个简单的事情：这个函数总是返回一个promise，如果代码中有return <非promise>语句，JavaScript会自动把返回的这个value值包装成promise的resolved值。

# Promise语法
sleep = (ms) ->
  new Promise (resolve) ->
    global.setTimeout(resolve, ms)

countdown = (second) ->
  for i in [second..1]
    if i > 5
      console.time("No.#{i} 计数时间：")
      await sleep(1000)
      # console.log "  #{new Date()}"
      t2 = +new Date()
      console.timeEnd("No.#{i} 计数时间：")
    else
      await sleep(1000)
      console.log "No.#{i} Count in 1000ms"

countdown 8

# 异步样例 2
ajax = (t) ->
  new Promise (resolve) ->
    global.setTimeout (-> resolve(t + 200)), t

step1 = (t) ->
  console.log "SUBMIT step1 in #{t}ms"
  return ajax(t)
step2 = (t) ->
  console.log "SUBMIT step2 in #{t}ms"
  return ajax(t)
step3 = (t) ->
  console.log "SUBMIT step3 in #{t}ms"
  return ajax(t)

submit = () ->
  console.time('SUBMIT step total')
  t1 = 0
  t2 = await step1(t1)
  t3 = await step2(t2)
  result = await step3(t3)
  console.log "SUBMIT result is #{result}ms"
  console.timeEnd('SUBMIT step total')

submit()
```

##### 关于call与apply

```coffeescript
js
// js apply
method.apply(this,arguments);

//coffee
method.apply @, arguments
//或
caller: ->
  @method arguments...

//  caller: function() {
//   return this.method.apply(this, arguments);
// }
```

##### XMLHttpRequest Code Snippets

```coffeescript
### Post request example POST请求
# create new request
xhr = new XMLHttpRequest
# set HTTP verb and URL
xhr.open('POST', '', true)
# define content type as JSON since we are sending a JSON string
xhr.setRequestHeader('Content-Type', 'application/json');
# define callback
xhr.onreadystatechange = () ->
    if xhr.readyState==4 and xhr.status==200
    document.write(xhr.responseText)

# send stringified data
xhr.send(JSON.stringify({
    "details": ["name"]
}));
```

💊 异步函数处理 async,使用NODEJS 时 -coffeeScript 2.0

```
### 异步函数处理 async,使用NODEJS 时 -coffeeScript 2.0
# 制造一个 promise， 使用 promise 执行异步
sleep = (ms) ->
  new Promise (resolve) ->
    # nodejs window = global
    window.setTimeout resolve, ms

countdown = (seconds) ->
  for i in [seconds..1]
    console.log "#{i} count"
    await sleep 1000 # 异步函数测试

countdown 30
```






## 在node环境下使用coffee



方法1:  coffeescript 2 参考 [Coffee官方文档](http://coffeescript.org/#nodejs-usage) 

coffeeScript 1.0 的node  [参考文档](http://www.marcusoft.net/2015/03/node-with-coffeescript-not-a-piece-of-cake.html) 

node模块的coffeescript[ 参考文档](http://nickdesaulniers.github.io/blog/2013/08/28/making-great-node-dot-js-modules-with-coffeescript/)



方法2: 最简单的用法:

1.`npm install coffeescript`

1. 在package.json里的script 加上`  "start": "coffee index.coffee --nodejs"`, 意味着直接用coffee运行,不用管js;

或者, 使用bash运行:  `coffee -w -c *`**`.coffee` , 然后在package.json里的script 里指定使用编译后的**`***.js`.

coffee2的编译注意可以是ES6.



## CoffeeScript 风格指南

[参考](https://cnodejs.org/topic/53a64eb1c3ee0b5820b20863)来源

#### 代码布局（Code Layout）

#### Tab 还是 空格？（Tabs or Spaces?）

只用 **空格**，每级缩进均为 **2 个空格**。切勿混用 Tab 和空格。

#### 最大行宽（Maximum Line Length）

限制每行最多 79 个字符。

#### 空行（Blank Lines）

顶级函数和类的定义用一个空行分开。

类内部的函数定义也用一个空行分开。

对于每个函数体内，只在为了提高可读性的情况下才使用一个空行（例如：为了达到划分逻辑的目的）。

#### 结尾空白（Trailing Whitespace）

不要在任何一行保留行尾空白。

#### 可选的逗号（Optional Commas）

当对象（或数组）的属性（或元素）作为单独一行列出时，避免在换行符前使用逗号。如下：

```coffeescript
# 好
foo = [
  'some'
  'string'
  'values'
]
bar:
  label: 'test'
  value: 87

# 差
foo = [
  'some',
  'string',
  'values'
]
bar:
  label: 'test',
  value: 87
```

#### 编码（Encoding）

UTF-8 是首选的源文件编码。

### 模块导入（Module Imports）

如果需要导入模块 \(CommonJS 模块，AMD，等等.\), `require` 语句应该单独作为一行。如下：

```coffeescript
require 'lib/setup'
Backbone = require 'backbone'
{ Backbone } = require 'backbone'
```

这些语句应该按以下顺序去分组：

1. 标准库的导入 _（如果标准库存在）_
2. 第三方库的导入
3. 本地导入 _（导入这个应用程序的或库的具体依赖）_

### 表达式和语句中的空白（Whitespace in Expressions and Statements）

下列情况应该避免多余的空格：

* 紧贴着圆括号、方括号和大括号内部

  ```coffeescript
       ($ 'body') # 好
       ( $ 'body' ) # 差
  ```

* 紧贴在逗号前

  ```coffeescript
       console.log x, y # 好
       console.log x , y # 差
  ```

额外建议：

* 在下列二元操作符的左右两边都保留 **一个空格**

  * 赋值运算符: `=`

    * _注意这同样适用于函数定义中的默认参数_

      ```coffeescript
       test: (param = null) -> # 好
       test: (param=null) -> # 差
      ```

  * 自增运算符: `+=`, `-=`, 等等。

  * 比较运算符: `==`, `<`, `>`, `<=`, `>=`, `unless`, 等等。

  * 算术运算符: `+`, `-`, `*`, `/`, 等等。

  * _（这些操作符两边的空格不要多于一个）_

    ```coffeescript
         # 好
         x = 1
         y = 1
         fooBar = 3

         # 差
         x      = 1
         y      = 1
         fooBar = 3
    ```

#### 注释（Comments）

如果你修改了一段已有注释说明的代码，则也要更新它对应的注释。（理想状态是，重构这段代码直到它不需要注释说明，然后再把之前的注释全删掉。）

注释的首字母要大写，除非第一个单词是以小写字母开头的标识符。

如果注释很短，可以省略末尾的句号。

#### 块注释（Block Comments）

注释块通常应用于尾随其后的一段代码。

每一行注释都以 `#` 加一个空格开头，而且和被注释的代码有相同的缩进层次。

注释块内的段落以仅含单个 `#` 的行分割。

```coffeescript
  # 这是一个块注释。请注意假如这是一段块注释，
  # 则它描述的就应该是接下来的这段代码。
  #
  # 这是块注释的第二段。
  # 请注意这段是由上一行带有 # 号的空行分开的。（P.S. 最好用英文写注释）

  init()
  start()
  stop()
```

### 行内注释（Inline Comments）

行内注释紧贴在被描述的代码的上一行，如果行内注释足够短，则可以处在同一行行尾（由一个空格隔开）。

所有行内注释都以 `#` 加一个空格开头。

应该限制行内注释的使用，因为它们的存在通常是一个代码异味的标志。

不要给显而易见的情况作行内注释：

```coffeescript
  # 差
  x = x + 1 # x 自增
```

然而，行内注释在某些情况下是有用的：

```coffeescript
  # 好
  x = x + 1 # 边界补足
```

#### 命名规范（Naming Conventions）

使用 `小驼峰命名法` （第一个词的首字母小写，后面每个词的首字母大写）来命名所有的变量、方法和对象属性。

使用 `大驼峰命名法` （第一个词的首字母，以及后面每个词的首字母都大写）来命名所有的类 _（在\[其他类似的命名法\]\[camel-case-variations\]中，这种风格通常也被称为 _`帕斯卡命名法（PascalCase）`_、 _`大写驼峰命名法（CamelCaps）`_ 或 _`首字母大写命名法（CapWords）`_。）_

_（CoffeeScript **官方**  约定是用驼峰命名法，因为这可以简化与 JavaScript 的相互转化，想了解更多，请看\[这里\]\[coffeescript-issue-425\].\)_

对于常量，单词全部大写，用下划线隔开即可：

```coffeescript
CONSTANT_LIKE_THIS
```

私有函数和私有变量都应该在前面加一个下划线：

```coffeescript
_privateMethod: ->
```

#### 函数（Functions）

_（以下这些准则同样适用于类中的方法。）_

当声明一个带参函数时，应在参数列表的右圆括号后空出一个空格：

```coffeescript
foo = (arg1, arg2) -> # 好
foo = (arg1, arg2)-> # 差
```

无参函数不要用圆括号：

```coffeescript
bar = -> # 好
bar = () -> # 差
```

当函数链式调用，却在一行放不下时，则把每个函数调用都另起一行，且都缩进一级（即在 `.` 前加两个空格）。

```coffeescript
[1..3]
  .map((x) -> x * x)
  .concat([10..12])
  .filter((x) -> x < 11)
  .reduce((x, y) -> x + y)
```

当调用函数时，我们应该为了提高可读性而去掉圆括号。请记住，「可读性」是我们主观臆断的。只有类似下面几个例子的情况才被社区认为是最佳的：

```coffeescript
baz 12

brush.ellipse x: 10, y: 20 # 大括号在适当的时候也可以去掉

foo(4).bar(8)

obj.value(10, 20) / obj.value(20, 10)

print inspect value

new Tag(new Value(a, b), new Arg(c))
```

有时候你会发现圆括号用来包裹的是函数体（而不是函数的参数）。请看下面的例子（以下简称为「函数体风格」）：

```coffeescript
($ '#selektor').addClass 'klass'

(foo 4).bar 8
```

这段代码会编译为：

```coffeescript
$('#selektor').addClass 'klass'

foo(4).bar 8
```

一些习惯链式调用的人会巧用「函数体风格」进行单独初始化：

```coffeescript
($ '#selektor').addClass('klass').hide() # 单独初始化调用
(($ '#selektor').addClass 'klass').hide() # 全部调用
```

「函数体风格」并不得到推荐。但是， **当它适应一些特殊的项目需求时，还是得用它。**

#### 字符串（Strings）

用字符串插值代替字符串连接符：

```coffeescript
'this is an #{adjective} string' # 好
'this is an ' + adjective + ' string' # 差
```

最好用单引号 \(`''`\) 而不是双引号 \(`""`\) 。除非是插入到另一段现有的字符串中（类似字符串插值）。

#### 条件判断（Conditionals）

用 `unless` 来代替 `if` 的否定情况。

不要用 `unless...else`， 而用 `if...else`:

```coffeescript
  # 好
  if true
    ...
  else
    ...

  # 差
  unless false
    ...
  else
    ...
```

多行的 if/else 语句应该缩进：

```coffeescript
  # 好
  if true
    ...
  else
    ...

  # 差
  if true then ...
  else ...
```

####  循环和列表解析（Looping and Comprehensions）

尽可能的使用列表解析：

```coffeescript
  # 好
  result = (item.name for item in array)

  # 差
  results = []
  for item in array
    results.push item.name
```

还可以过滤结果：

```coffeescript
result = (item for item in array when item.name is "test")
```

遍历对象的键值：

```coffeescript
object = one: 1, two: 2
alert("#{key} = #{value}") for key, value of object
```

### 扩展本地对象（Extending Native Objects）

不要修改本地对象。

比如，不要给 `Array.prototype` 引入 `Array#forEach` 。

### 异常（Exceptions）

不要抑制异常抛出。

### 注解（Annotations）

必要的时候应该写注解，来指明接下来的代码块具体将干什么。

注解应紧贴在被描述代码的上一行。

注解关键字后面应该跟一个冒号加一个空格，加一个描述性的注释。

```coffeescript
  # FIXME: The client's current state should *not* affect payload processing.
  resetClientState()
  processPayload()
```

如果注解不止一行，则下一行缩进两个空格。

```coffeescript
  # TODO: Ensure that the value returned by this call falls within a certain
  #   range, or throw an exception.
  analyze()
```

注解有以下几类：

* `TODO`: 描述缺失的功能，以便日后加入
* `FIXME`: 描述需要修复的代码
* `OPTIMIZE`: 描述性能低下，或难以优化的代码
* `HACK`: 描述一段值得质疑（或很巧妙）的代码
* `REVIEW`: 描述需要确认其编码意图是否正确的代码

如果你必须自定义一个新的注解类型，则应该把这个注解类型记录在项目的 README 里面。

### 其他（Miscellaneous）

`and` 更优于 `&&`.

`or` 更优于 `||`.

`is` 更优于 `==`.

`not` 更优于 `!`.

`or=` 应在可能的情况下使用：

```coffeescript
temp or= {} # 好
temp = temp || {} # 差
```

最好用 \(`::`\) 访问对象的原型：

```coffeescript
Array::slice # 好
Array.prototype.slice # 差
```

最好用 `@property` 而不是 `this.property`.

```coffeescript
return @property # 好
return this.property # 差
```

但是，避免使用 **单独的** `@`:

```coffeescript
return this # 好
return @ # 差
```

没有返回值的时候避免使用 `return` ，其他情况则需要显示 return 。

当函数需要接收可变数量的参数时，使用 splats  \(`...`\)。

```coffeescript
console.log args... # 好

(a, b, c, rest...) -> # 好
```




