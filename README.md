# zigbee-docs

## Node进程间通信可供选择的方案
- (Pub/Sub–Redis)[http://redis.io/topics/pubsub]
- (dnode)[https://github.com/substack/dnode] 
- (zmq)[https://github.com/JustinTulloss/zeromq.node]
- tcp
- http

### 总结
- (dnode)[https://github.com/substack/dnode]适合异步双向远程方法调用.
- (zmq)[https://github.com/JustinTulloss/zeromq.node]
性能非常强大,如果要选择使用TCP通信,那么宁可使用(zmq)[https://github.com/JustinTulloss/zeromq.node],TCP通信是单对单的,(zmq)[https://github.com/JustinTulloss/zeromq.node]还可以做到多对多.
- Pub/Sub–Redis订阅和发布.




### 参考文献
- (ZMQ指南)[https://github.com/anjuke/zguide-cn]

## Express端实时推送
- (WebSocket-Node)[https://github.com/theturtle32/WebSocket-Node]
- (socket.io)[https://github.com/socketio/socket.io]



## 初步产用方案

### 客户端和http服务端

由于下位机每1s钟(时间可能更长)轮询一次,先尝试产用Redis实现, 浏览器端发送HTTP请求(这个请求会被长时间保持?Socket.io?)给服务端,服务端产用Redis缓存建立命令池. 

### Tcp服务端和http服务端

- 轮询命令

如果是查询是否有浏览器端发送的命令,Tcp接收到下位机的通讯时直接读取Redis查看有没有浏览器端的命令.有命令直接向下位机发送命令.

- 上传数据

当命令下发请求查询下位机数据或者下位机需要主动上传数据时,Tcp通过Redis的发布机制将数据发送给Express,Express通过Redis的订阅机制获取数据,并通过Socket.io将数据实时推送给客户端.

