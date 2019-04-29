# 代理与爬虫

## 常用动态浏览器头： userAgent.js

```
const userAgents = [
  'Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.8.0.12) Gecko/20070731 Ubuntu/dapper-security Firefox/1.5.0.12',
  'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0; Acoo Browser; SLCC1; .NET CLR 2.0.50727; Media Center PC 5.0; .NET CLR 3.0.04506)',
  'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.56 Safari/535.11',
  'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_3) AppleWebKit/535.20 (KHTML, like Gecko) Chrome/19.0.1036.7 Safari/535.20',
  'Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.9.0.8) Gecko Fedora/1.9.0.8-1.fc10 Kazehakase/0.5.6',
  'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.71 Safari/537.1 LBBROWSER',
  'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0; .NET CLR 3.5.30729; .NET CLR 3.0.30729; .NET CLR 2.0.50727; Media Center PC 6.0) ,Lynx/2.8.5rel.1 libwww-FM/2.14 SSL-MM/1.4.1 GNUTLS/1.2.9',
  'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 1.1.4322; .NET CLR 2.0.50727)',
  'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E; QQBrowser/7.0.3698.400)',
  'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; QQDownload 732; .NET4.0C; .NET4.0E)',
  'Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:2.0b13pre) Gecko/20110307 Firefox/4.0b13pre',
  'Opera/9.80 (Macintosh; Intel Mac OS X 10.6.8; U; fr) Presto/2.9.168 Version/11.52',
  'Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.8.0.12) Gecko/20070731 Ubuntu/dapper-security Firefox/1.5.0.12',
  'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E; LBBROWSER)',
  'Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.9.0.8) Gecko Fedora/1.9.0.8-1.fc10 Kazehakase/0.5.6',
  'Mozilla/5.0 (X11; U; Linux; en-US) AppleWebKit/527+ (KHTML, like Gecko, Safari/419.3) Arora/0.6',
  'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E; QQBrowser/7.0.3698.400)',
  'Opera/9.25 (Windows NT 5.1; U; en), Lynx/2.8.5rel.1 libwww-FM/2.14 SSL-MM/1.4.1 GNUTLS/1.2.9',
  'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36'
]

module.exports = userAgents

```
app.js

```

import request from 'superagent'
import userAgents from '../src/userAgent'

async function doRequest(){
	let userAgent = userAgents[parseInt(Math.random() * userAgents.length)]
	request.get('http://www.xxx.com')
    .set({ 'User-Agent': userAgent })
    .timeout({ response: 5000, deadline: 60000 })
    .end(async(err, res) => {
      // 处理数据
    })
}

```

## 动态ip

设置动态IP需要用到一个superagent插件 `superagent-proxy`，除此之外为了避免每次爬取时都去获取一次动态IP的列表，我将爬取到的动态IP列表存放在redis中，并设置10分钟的过期时间。数据过期之后再重新发送获取动态IP的请求。
ps: 这里我使用的动态IP是爬虫网络科技公司提供的免费代理，因为免费所以难免会有些缺陷。有时候使用他的代理ip并不能访问得通，我在后面会做单独的处理。

app.js
```
import request from 'superagent'
import requestProxy from 'superagent-proxy'
import redis from 'redis'
// superagent添加使用代理ip的插件
requestProxy(request)
// redis promise化
bluebird.promisifyAll(redis.RedisClient.prototype)
bluebird.promisifyAll(redis.Multi.prototype)
// 建立mongoose和redis连接
const redisClient = connectRedis()

/**
 * 初始化redis
 */
function connectRedis() {
  let client = redis.createClient(config.REDIS_URL)
  client.on("ready", function(err) {
    console.log('redis连接 √')
  })
  client.on("error", function(err) {
    console.log(`redis错误，${err}  ×`);
  })
  return client
}


/**
 * 请求免费代理，读取redis，如果代理信息已经过期，重新请求免费代理请求
 */
async function getProxyIp() {
  // 先从redis读取缓存ip
  let localIpStr = await redisClient.getAsync('proxy_ips')
  let ips = null
  // 如果本地存在，则随机返回其中一个ip，否则重新请求
  if (localIpStr) {
    let localIps = localIpStr.split(',')
    return localIps[parseInt(Math.random() * localIps.length)]
  } else {
    let ipsJson = (await request.get('http://api.pcdaili.com/?orderid=888888888&num=100&protocol=1&method=1&an_ha=1&sp1=1&sp2=1&format=json&sep=1')).body
    let isRequestSuccess = false
    if (ipsJson && ipsJson.data.proxy_list) {
      ips = ipsJson.data.proxy_list
      isRequestSuccess = true
    } else {
      ips = ['http://127.0.0.1']
    }
    // 将爬取结果存入本地，缓存时间10分钟
    if (isRequestSuccess) {
      redisClient.set("proxy_ips", ips.join(','), 'EX', 10 * 60)
    }
    return ips[parseInt(Math.random() * ips.length)]
  }
}

async function doRequest(){
  let userAgent = userAgents[parseInt(Math.random() * userAgents.length)]
  let ip = await getProxyIp()
  let useIp = 'http://' + ip
  request.get('http://www.xxx.com')
    .set({ 'User-Agent': userAgent })
    .timeout({ response: 5000, deadline: 60000 })
    .proxy(ip)
    .end(async(err, res) => {
      // 处理数据
    })
}

```

之前说爬虫网络科技的免费ip有些缺陷—代理成功率有些低。这点必须想办法去修复，原理其实很简单，既然一次不成功那我就换个IP再试，直到成功了我才去开始执行解析html的逻辑

```
async function doRequest(){
  let userAgent = userAgents[parseInt(Math.random() * userAgents.length)]
  let ip = await getProxyIp()
  let useIp = 'http://' + ip
  request.get('http://www.xxx.com')
    .set({ 'User-Agent': userAgent })
    .timeout({ response: 5000, deadline: 60000 })
    .proxy(ip)
    .end(async(err, res) => {
      if (err) {
        console.log(`爬取页面失败，${err}，正在重新寻找代理ip... ×`)
        // 如果是代理ip无法访问，另外选择一个代理
        doRequest('http://' + await getProxyIp(), userAgents[parseInt(Math.random() * userAgents.length)])
        return
      }
      // 解析html
      console.log('爬取页面  √')
      await parseDivision(res.text)
    })
}
```


## 透明代理、匿名代理、混淆代理、高匿代理有什么区别？

这4种代理，主要是在代理服务器端的配置不同，导致其向目标地址发送请求时，REMOTE_ADDR， HTTP_VIA，HTTP_X_FORWARDED_FOR三个变量不同。

1、透明代理(Transparent Proxy)

- REMOTE_ADDR = Proxy IP
- HTTP_VIA = Proxy IP
- HTTP_X_FORWARDED_FOR = Your IP
透明代理虽然可以直接“隐藏”你的IP地址，但是还是可以从HTTP_X_FORWARDED_FOR来查到你是谁。

2、匿名代理(Anonymous Proxy)

- REMOTE_ADDR = proxy IP
- HTTP_VIA = proxy IP
- HTTP_X_FORWARDED_FOR = proxy IP
匿名代理比透明代理进步了一点：别人只能知道你用了代理，无法知道你是谁。

还有一种比纯匿名代理更先进一点的：混淆代理，见下节。

3、混淆代理(Distorting Proxies)

- REMOTE_ADDR = Proxy IP
- HTTP_VIA = Proxy IP
- HTTP_X_FORWARDED_FOR = Random IP address

如上，与匿名代理相同，如果使用了混淆代理，别人还是能知道你在用代理，但是会得到一个假的IP地址，伪装的更逼真：-）

4、高匿代理(Elite proxy或High Anonymity Proxy)

- REMOTE_ADDR = Proxy IP
- HTTP_VIA = not determined
- HTTP_X_FORWARDED_FOR = not determined
- 可以看出来，高匿代理让别人根本无法发现你是在用代理，所以是最好的选择。

## 常见代理类型

- Http代理：最常用的代理，代理代理客户机的http访问，主要代理浏览器访问网页，它的端口一般为80、8080、3128等。
- SSL代理也叫HTTPS代理：支持最高128位加密强度的http代理，可以作为访问加密网站的代理。加密网站是指以https//开始的网站。ssl的标准端口为443。
- HTTP CONNECT代理：允许用户建立TCP连接到任何端口的代理服务器，这种代理不仅可用于HTTP，还包括FTP、IRC、RM流服务等。 
- FTP代理：代理客户机上的ftp软件访问ftp服务器，其端口一般为21、2121。
- POP3代理：代理客户机上的邮件软件用pop3方式收邮件，其端口一般为110。
- Telnet代理：能够代理通信机的telnet，用于远程控制，入侵时经常使用。其端口一般为23。
- Socks代理：是全能代理，就像有很多跳线的转接板，它只是简单地将一端的系统连接到另外一端。支持多种协议，包括http、ftp请求及其它类型的请求。它分socks 4 和socks 5两种类型，socks 4只支持TCP协议而socks 5支持TCP/UDP协议，还支持各种身份验证机制等协议。其标准端口为1080。
- TUNNEL代理：经HTTPTunnet程序转换的数据包封装成http请求（Request）来穿透防火墙，允许利用HTTP服务器做任何TCP可以做的事情，功能相当于Socks5。
- 文献代理：可以用来查询数据库的代理，通过这些代理，可以获得互联网的相关科研学术的数据库资源，例如查询Sciencedirect网站（简称SD）、Academic Press、IEEE，SPRINGER等数据库。
- 教育网代理：指学术教育机构局域网通过特定的代理服务器可使无出国权限或无访问某IP段权限的计算机访问相关资源。
- 跳板代理：应用于跳板程序，可以看作一种具有动态加密的特殊socks5代理，，也可直接用于PSD软件。其端口一般为1813。 
- Ssso代理：代理客户机上的ssso程序访问远程网站，具有SSL加密强度的超级代理，支持socks。
- Flat代理：代理客户机上的flatsurfer程序访问远程网站，具有高强度加密数据流的特殊代理，支持socks，最大可设置三次级联，可以设置穿越代理。其端口一般为6700。 
- SoftE代理：代理客户机上的SoftEther程序访问远程网站，应用虚拟集线器HUB和虚拟网卡技术，具备VPN功能及多种认证方式的代理，符合https协议。

## 常用代理列表
[Zhihu:说说代理IP哪家好？](https://www.zhihu.com/question/55807309)
http://stormproxies.com
