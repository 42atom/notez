# NodeJs

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



