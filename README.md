# pomelo-admin-web-new
new pomelo-admin-web
pomelo-admin-web is a web console for [pomelo](https://github.com/NetEase/pomelo). it is based on [pomelo-admin](https://github.com/NetEase/pomelo-admin). it is just an web console example, you can implement your style like web console.

## Installation


```
git clone https://github.com/whtiehack/pomelo-admin-web-new.git
cd pomelo-admin-web-new
npm i -d
```


## Usage
just run

```
node app.js
```

open browser in your computer,and enjoy it

# Docker Image

[![](https://images.microbadger.com/badges/image/smallwhite/pomelo-admin-web.svg)](https://microbadger.com/images/smallwhite/pomelo-admin-web "Get your own image badge on microbadger.com")

[![](https://images.microbadger.com/badges/version/smallwhite/pomelo-admin-web.svg)](https://microbadger.com/images/smallwhite/pomelo-admin-web "Get your own version badge on microbadger.com")


```
docker run --name pinusadminweb -d --network host -e ADMIN_USERNAME=admin -e ADMIN_PASSWORD=admin   smallwhite/pomelo-admin-web
```


> 示例
```
docker run --name pinusadminweb3005 -d -p 7001:7001 --add-host dockerhost:`/sbin/ip route|awk '/default/ { print  $3}'` -e SERVER_HOST=192.168.1.10 -e ADMIN_PORT=7001 -e SERVER_PORT=3005 -e ADMIN_USERNAME=admin -e ADMIN_PASSWORD=admin   smallwhite/pomelo-admin-web

> ceshi2
docker run --name pinusadminweb3006 -d -p 7002:7002 --add-host dockerhost:`/sbin/ip route|awk '/default/ { print  $3}'` -e SERVER_HOST=192.168.1.10 -e ADMIN_PORT=7002 -e SERVER_PORT=3006 -e ADMIN_USERNAME=admin -e ADMIN_PASSWORD=admin   smallwhite/pomelo-admin-web
```

> 参数 `--network host` 目的是直接使用宿主机的网络。  如果去掉的话，那么启动容器的时候需要自己配置好环境变量。

## 更多的环境变量看这里：
```
adminConfig.username = process.env.ADMIN_USERNAME ? process.env.ADMIN_USERNAME : adminConfig.username;
adminConfig.password = process.env.ADMIN_PASSWORD ? process.env.ADMIN_PASSWORD : adminConfig.password;
adminConfig.port = process.env.ADMIN_PORT ? process.env.ADMIN_PORT : adminConfig.port;
adminConfig.host = process.env.ADMIN_HOST ? process.env.ADMIN_HOST : adminConfig.host;

config.host = process.env.SERVER_HOST ? process.env.SERVER_HOST : config.host;
config.port = process.env.SERVER_PORT ? process.env.SERVER_PORT : config.port;
config.username = process.env.SERVER_USERNAME ? process.env.SERVER_USERNAME : config.username;
config.password = process.env.SERVER_PASSWORD ? process.env.SERVER_PASSWORD : config.password;

```

## onlineUserModule


 onlineUserModule里放的是在线用户的module，对应的是服务器里面放的位置

说明

服务器需安装 [pomelo-admin](https://github.com/NetEase/pomelo-admin)库


```
npm install --save  pomelo-admin
```



#### 经过测试onlineUser里面的address能获取websocket协议服务器的用户ip，而socke.io协议的不能；另外，其他的如systemInfo等由pomelo-admin提供的module windows系统不能获取信息，linux系统可以。


### 流程记录
# 连接master服务器(app.js <--> master)
1.const adminClient = new admin.adminClient({
    username:config.username,
    password:config.password
});

2.adminClient.connect('pomelo-web-1',config.host,config.port,function(err){
})

# 访问web服务器(views.index.html <--> app.js / client.js)
1.var client = window.client = new ConsoleClient({
	username: username,
	password: password,
	md5: false
});

2.client.connect('browser-----' + Date.now(), host, port, function(err){
});

3.io.on('connection', function (socket) {
    socket.emit('connect', { hello: 'world' });
    socket.on('client',function(req){
        adminClient.request(req.moduleId,req.body,function(err,data,msg){})
    })
});