# snippets

💊  关于call与apply

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

💊 XMLHttpRequest Code Snippets

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

💊

```

```

💊

```

```

💊

```

```

💊

