---
title: socket简单上手
date: 2021-12-26 22:43:04
tags: socket
categories:
- Socket
---
## socket 简单上手

> 利用express和socket实现一个简易的聊天室 日后可持续添加新功能

### 环境搭建

只需基于node的express框架搭建出一个包含表单和消息列表的html页面

```js
npm init -y  // 初始化package.json文件以安装node框架
npm i express  // 安装express框架
```

### 创建服务后台

利用express提供的能力搭建后台 

- 初始化 app 作为 HTTP 服务器的回调函数
- 监听3000端口 
- 定义了首页路由 **/** 

```js
// index.js
var app = require('express')();
var http = require('http').Server(app);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

### 添加socket.io

安装socket.io
```js
npm i socket.io
```

#### 在index.js中添加socket模块

- 通过传入http对象初始化了一个socket.io的实例 
- 监听connection事件来接收sockets，并将连接信息打印在console

```js
var io = require('socket.io')(http);

io.on('connection', function(socket){
  console.log('a user connected');
});
```

#### 在index.html 的 </body> 标签中添加socket-io-client

socket.io-client 暴露了一个 io 全局变量，然后连接服务器

```js
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io(); // 请注意我们在调用 io() 时没有指定任何 URL,因为它默认将尝试连接到提供当前页面的主机
  // io这里可以传入参数（可以log一下参数），参数是当前Socket的所有信息
</script>
```

### 监听connection和disconnect事件

```js
io.on('connection', function(socket){
  // 这里的socket可以拿到socket相关参数 比如每个socket连接的id 区分不同连接的用户
  console.log('a user connected');
  socket.on('disconnect', function(){
    console.log('user disconnected');
  });
});
```

### 手动触发自定义事件

> 比如发送消息

当用户输入消息时，服务器接收一个 chat message 事件 
此处类似vue的事件触发，socket.emit 触发自定义事件并传入参数

#### client端
```js
<script src="/socket.io/socket.io.js"></script>
<script src="https://code.jquery.com/jquery-1.11.1.js" rel="external nofollow" ></script>
<script>
  $(function () {
    var socket = io();
    $('form').submit(function(){
      socket.emit('chat message', $('#m').val());
      $('#m').val('');
      return false;
    });
  });
</script>
```

#### server端

监听client端的自定义事件：此处事件名为 chat message

```js
io.on('connection', function(socket){
  socket.on('chat message', function(msg){
    console.log('message: ' + msg);
  });
});
```

### socket也提供了广播功能

让服务器将消息发送给其他用户

要将事件发送给每个用户，Socket.IO 提供了 io.emit 方法：

```js
io.emit('some event', { for: 'everyone' });
```

要将消息发给除特定 socket 外的其他用户，可以用 broadcast 标志

```js
io.on('connection', function(socket){
  socket.broadcast.emit('hi');
});
```

#### 在client端监听

```js
socket.on('chat message', function(msg){
  $('#messages').append($('<li>').text(msg));
});
```

至此：简单的socket上手已完成