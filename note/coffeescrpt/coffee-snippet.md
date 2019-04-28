# coffee snippet
```coffeescript
# 阶乘写法，非尾递归
f = (n) ->
  if n is 1
    1
  else
    n * f(n - 1)

console.log '阶乘 = ' + result = f(6)
console.log result = 1 * 2 * 3 * 4 * 5 * 6

# 汉诺塔样例
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



