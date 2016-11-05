# STOMP
[Reference](http://stomp.github.io/stomp-specification-1.2.html)

STOMP is a simple interoperable protocol designed for asynchronous message passing between clients via mediating servers.

** CAN ACT AS: **

-As a ** producer **, sending messages to a destination on the server via a SEND frame
-as a consumer, sending a SUBSCRIBE frame for a given destination and receiving messages from the server as MESSAGE frames.

##### 1.THE FRAME 

```
COMMAND				//It opens with Command String +EOL
header1:value1		//Header 1 + EOL
header2:value2		//Header 2 etc…  
					//A space separate the headers from the body
Body^@				//Body + closing
```



//All the values are encoded as UTF8
//Escaping is very important , CONNECT and CONNECTED doesn’t automatically escape them

* \r (octet 92 and 114) translates to carriage return (octet 13)
* \n (octet 92 and 110) translates to line feed (octet 10)
* \c (octet 92 and 99) translates to : (octet 58)
* \\ (octet 92 and 92) translates to \ (octet 92)

// clients and servers MUST NEVER trim or pad headers with spaces.

// 2.BODY
Only SEND, MESSAGE and ERROR  can have Body, other Command strings only have headers

//Standard Headers are used by everyone, they are generic

	content-length 			//An octet count of the length of the body   (octet = 1 byte = 8 bits)
						//Need to be always specified

	content-type			//header to help the receiver of the frame interpret its body
						//If the content is not compatible with the declared content-type
						// it will be considered a binary blob
						// e.g.  content-type: text/html;charset=utf-16