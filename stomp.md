## STOMP
<hr>

STOMP is a simple interoperable protocol designed for asynchronous message passing between clients via mediating servers.

**CAN ACT AS:**

-As a **producer**, sending messages to a destination on the server via a SEND frame

-as a **consumer**, sending a SUBSCRIBE frame for a given destination and receiving messages from the server as MESSAGE frames.

##### 1. THE FRAME 

```
COMMAND				//It opens with Command String +EOL
header1:value1		//Header 1 + EOL
header2:value2		//Header 2 etc…  
					//A space separate the headers from the body
Body^@				//Body + closing
```



//All the values are encoded as UTF8
//Escaping is very important , CONNECT and CONNECTED doesn’t automatically escape them

* `\r` (octet 92 and 114) translates to carriage return (octet 13)
* `\n` (octet 92 and 110) translates to line feed (octet 10)
* `\c` (octet 92 and 99) translates to : (octet 58)
* `\\` (octet 92 and 92) translates to \ (octet 92)

// clients and servers MUST NEVER trim or pad headers with spaces.

##### 2. BODY

Only `SEND`, `MESSAGE` and `ERROR`  can have Body, other Command strings only have headers. Standard Headers are used by everyone, they are generic.


Header **content-length**	
	
An octet count of the length of the body .(octet = 1 byte = 8 bits). Need to always be specified

Header **content-type**	

Header to help the receiver of the frame interpret its Body
( *e.g.  content-type: text/html;charset=utf-16* )

Header **receipt**	
	
Any client frame other than `CONNECT` Mat specify a `receipt` header with an arbitrary value. This will help the server to *Aknowledge* the processing of the client with a `RECEIPT` frame

```
SEND
destination:/queue/a
receipt:message-1234

hello queue a^@
```			
	
STOMP will ignore repeated Headers, only the first will be used. You can keep previous as history

```
MESSAGE
foo:World  //This will be used
foo:Hello

```
**Size Limits**

Server should limit maximums on:
* Number of frame headers allowed in a single Frame
* Maximum length of Header lines
* Maximum Size of frame body


##### 3. CONNECTNG

STOMP client initiate

```
CONNECT
accept-version:1.0,1.1,2.0  //the versions Client can handle
host:stomp.github.org

^@
```
If the server accepts it returns:

```
CONNECTED
version:1.1

^@
```
	
If rejected , the server should return an `ERROR` frame

```
ERROR
version:1.2,2.1
content-type:text/plain

Supported protocol versions are 1.2 2.1^@

```

STOMP **clients** must use the following headers:

* `accept-version`
* `host`
* `login`  //optional
* `passcode` //optional
* `heart-beat` //optional

	
STOMP **servers**

* `version`
* `heart-beat`
* `session`
* `server`  //e.g. server: Apacje/1.3.9


<hr>

#### 4.Client Frames

* SEND
* SUBSCRIBE
* UNSUBSCRIBE
* BEGIN
* COMMIT
* ABORT
* ACK
* NACK
* DISCONNECT
			
			
	

<hr>						
References:

- [stomp-specification-1.2](http://stomp.github.io/stomp-specification-1.2.html)