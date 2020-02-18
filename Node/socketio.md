## Socket.io
Node製 socket.io サーバーとの通信のやり取りでチャットとか作るのに使う。  
使い方はEventEmitterに似てる。  
```
// Server
const app = require('express')();
const server = require('http').Server(app);
const io = require('socket.io')(server);

app.get('/', function(req, res){
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function(socket){
  console.log('a user connected');
  socket.on('eventName', arguments);
  io.emit('eventName', arguments);
});

server.listen(3000, function(){
  console.log('listening on *:3000');
});

// HTML
<script src="/socket.io/socket.io.js"></script>
<script>
	const socket = io();
  
  socket.emit('eventName', arguments);
  socket.on('eventName', arguments);
</script>
```