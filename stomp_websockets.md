## STOMP-WEBSOCKET
<hr>

This library is **not a pure STOMP client**. It is aimed to run on the WebSockets. This means that this library can not connect to regular STOMP brokers since they **would not understand the handshake initiated by the WebSocket** which is not part of the STOMP protocol and would likely reject the connection.

##### Installation
include `stomp.js ` build generated from Coffee script

##### Connect

```
var url = "ws://localhost:61614/stomp";
  var client = Stomp.client(url);
```
Additional Protocols can be included like: `Stomp.client(url, protocols)`

Can either be a single string or an array of strings to specify multiple subprotocols. 

##### Websocket Version

By default stomp.js will use the browser websocket version, however you can also override it with `Stomp.over(ws);`

##### Prepare connection to a websocket

```
< script src="http://cdn.sockjs.org/sockjs-0.3.min.js"> </script>
< script>
  // use SockJS implementation instead of the browser's native implementation
    var ws = new SockJS(url);
    var client = Stomp.over(ws);
    [...]
  </script>
```

Use `Stomp.client(url)` to use regular WebSockets or use `Stomp.over(ws)` if you required another type of WebSocket. 
  
  
To connect to a STOMP broker over a Web Socket, use instead the Stomp.overWS(url) method: 
 
```
var client = Stomp.overWS('ws://localhost:61614/stomp');
```

##### CONNECT to STOMP over a websocket


-You need to use `connect();` and pass 2 arguments `login` and `passcode`. 

**-Behind the scene, the client will open a connection using a WebSocket and send a CONNECT frame.**


The connection is done asynchronously: you have no guarantee to be effectively connected when the call to connect returns. To be notified of the connection, you need to pass a connect_callback function to the connect() method:

```
var error_callback = function(error) {
    // display the error's message header:
    alert(error.headers.message);
  };
```
notice the error parameter that can trigger an alert (or a log or something else).


Other variants

```
client.connect(login, passcode, connectCallback);

  client.connect(login, passcode, connectCallback, errorCallback);
  
  client.connect(login, passcode, connectCallback, errorCallback, host);
  
```

to pass more parameters you can use a map (headers) . 

```
client.connect(headers, connectCallback);
  client.connect(headers, connectCallback, errorCallback);
  ```

```

var headers = {
      login: 'mylogin',
      passcode: 'mypasscode',
      // additional header
      'client-id': 'my-client-id'
    };
    client.connect(headers, connectCallback);
    
```

where header is a map and connectCallback and errorCallback are functions. 


##### DISCONNECT

```
client.disconnect(function() {
    alert("See you next time!");
  };
```


##### SEND MESSAGES

Connect using `send()` + 1 mandatory arguments: `destination` which is the URL that you are connecting to. It also takes 2 optional arguments:

*  `JS Object` containing additional STOMP message headers 
*  and `body`

```
client.send("/queue/test", {priority: 9}, "Hello, STOMP");
```

If you want to pass a `body` you need to pass `headers` argument. If you dont have additional headers to pass include an empty object like: `{}`

``` 
client.send(destination, {}, body);

```

##### SUBSCRIBE to receive messages

To receive messages in the browser, the STOMP client must first subscribe to a destination.


Suscribe using `subscribe()` + 2 mandatory arguments:


* `Destination`  a String corresponding to the destination
* `callback` a function with one message argument and an optional argument headers, a JavaScript object for additional headers. 

```
var subscription = client.subscribe("/queue/test", callback);
```
The callback look like this:

```
callback = function(message) {
    // called when the client receives a STOMP message from the server
    if (message.body) {
      alert("got message with body " + message.body)
    } else {
      alert("got empty message");
    }
  });
  
```
With this the Client is sending a **Subscribe** Frame to the server and registering the **callback** function.
When the server returns a message to the client, the client will in turn call the callback with a STOMP Frame object corresponding to the message.
```


In stomp you need to specify an ID for every message. This library do it for you automatically, or if you want you can do it  manually like this: 

```
var mysubid = '...';
  var subscription = client.subscribe(destination, callback, { id: mysubid });  
```
 
 It can also take additional STOMP headers:
 
 ```
 var headers = {ack: 'client', 'selector': "location = 'Europe'"};
  client.subscribe("/queue/test", message_callback, headers);
  ```

`'selector': "location = 'Europe'"` works as a filter

##### Unsubscribe

```
var subscription = client.subscribe(...);
  
  ...
  
  subscription.unsubscribe();
```

##### JSON SUPPORT

Use stringify to convert to an object and viceversa

```
var quote = {symbol: 'APPL', value: 195.46};
  client.send("/topic/stocks", {}, JSON.stringify(quote));

  client.subcribe("/topic/stocks", function(message) {
    var quote = JSON.parse(message.body);
    alert(quote.symbol + " is at " + quote.value);
  };
  ```

##### AKNOWLEDGEMENTS

By default, STOMP messages will be automatically acknowledged by the server before the message is delivered to the client.

But you can also do it manually

```
var subscription = client.subscribe("/queue/test",
    function(message) {
      // do something with the message
      ...
      // and acknowledge it
      message.ack();
    },
    {ack: 'client'}
  );
  ```















