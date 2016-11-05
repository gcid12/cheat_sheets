##WEBSOCKETS API
<hr>

New Connection

```
var connection = new WebSocket('ws://html5rocks.websocket.org/echo', ['soap', 'xmpp']);
```

When the connection is open, send some data to the server

```
connection.onopen = function () {
  connection.send('Ping'); // Send the message 'Ping' to the server
};
```

Log errors

```
connection.onerror = function (error) {
  console.log('WebSocket Error ' + error);
};
```

Log messages from the server

```
connection.onmessage = function (e) {
  console.log('Server: ' + e.data);
};
```

Setting binaryType to accept received binary as either 'blob' or 'arraybuffer'

```
connection.binaryType = 'arraybuffer';
connection.onmessage = function(e) {
  console.log(e.data.byteLength); // ArrayBuffer object if binary
};
```

Determining accepted extensions

```
console.log(connection.extensions);

```


<hr/>
References

[html5 Rocks](https://www.html5rocks.com/en/tutorials/websockets/basics/)