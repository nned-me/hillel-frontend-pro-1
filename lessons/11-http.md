# Internet
How does it all work?

![enter image description here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview/http-layers.png)


- CLIENT
- SERVER
- PROXY

Proxy chain:

![enter image description here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview/client-server-chain.png)

## Network Models

**OSI MODEL** - 7 LAYERS

**TCP/IP MODEL** - 4 LAYERS

![enter image description here](https://www.imperva.com/learn/wp-content/uploads/sites/13/2020/02/OSI-vs.-TCPIP-models.jpg.webp)





## TCP / IP
Transport connection protocol / Internet protocol

 ![enter image description here](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/UDP_encapsulation.svg/800px-UDP_encapsulation.svg.png?1632403188857)


 - **APPLICATION LAYER**  --- semantic level & protocols
	 - HTTP
	 - TLS/SSL
	 - SSH
	 - SMTP
	 - telnet
	 - ...

 - **TRASNPORT LAYER** ---  data stream
	 - open an close connections
	 - correct data errors
	 - resend missing data
	 - ...

 - **INTERNET LAYER** -- routing to destinations
	 - 	IP adresses,
	 - packages
	 - IPV4
	 - IPV6
	 - IPSec (VPN)
	 - ...

 - **LINK LAYER** --- physical link & devices protocol
	 - physical link
	 - wireless / wire
	 - MAC adresses
	 - WPA2 encryption
	 - ...





## HTTP / HTTPS

**HTTP** = **Hypertext Transfer Protocol**
**HTTPS** =  **Hypertext Transfer Protocol Secure**

**HTTPS** can be:
- **HTTP** over **TLS** -- new one
- **HTTP** over **SSL** -- old one

HTTP & HTTP/1.1 --- plain text, human readable
 HTTP/2 -- divided into frames, non-human readable



---
### TYPICAL BROWSER REQUEST
- **DNS** Lookup
	- local lookup
	- ISP lookup
	- Name servers
- Start **TCP** connection
- OR: Start **TLS** connection
- Send **HTTP Request**
- Server process request and forms response
- Server sends **HTTP Response**




---
### HTTP ANATOMY

  ![enter image description here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/httpmsg2.png)


![enter image description here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/httpmsgstructure2.png)


Request


    GET /home.html HTTP/1.1
    Host: developer.mozilla.org
    User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
    Accept-Language: en-US,en;q=0.5
    Accept-Encoding: gzip, deflate, br
    Referer: https://developer.mozilla.org/testpage.html
    Connection: keep-alive
    Upgrade-Insecure-Requests: 1
    If-Modified-Since: Mon, 18 Jul 2016 02:36:04 GMT
    If-None-Match: "c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"
    Cache-Control: max-age=0



**RESPONSE**

```
200 OK
Access-Control-Allow-Origin: *
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Mon, 18 Jul 2016 16:06:00 GMT
Etag: "c561c68d0ba92bbeb8b0f612a9199f722e3a621a"
Keep-Alive: timeout=5, max=997
Last-Modified: Mon, 18 Jul 2016 02:36:04 GMT
Server: Apache
Set-Cookie: mykey=myvalue; expires=Mon, 17-Jul-2017 16:06:00 GMT; Max-Age=31449600; Path=/; secure
Transfer-Encoding: chunked
Vary: Cookie, Accept-Encoding
X-Backend-Server: developer2.webapp.scl3.mozilla.com
X-Cache-Info: not cacheable; meta data too large
X-kuma-revision: 1085259
x-frame-options: DENY
```




 - **START LINE**
		1. **HTTP Method:**  GET
		2. **URL:** google.com
		3. **PROTOCOL**: HTTP/1.1
- **HEADERS**
	- common headers
	-
	- custom headers
- **BODY** -- data of any type:
	- text
	- json
	- xml
	- binary


---
### HTTP METHODS

- **GET** --- get some data
- **HEAD** -- same but headers only
- **POST** --- send some data to change
- **PUT** --- send data entirely
- **DELETE** -- delete some data
- **PATCH** --- update some data
- **OPTIONS** --- get available commands

Rare used:
- **CONNECT**
- **TRACE**


---
### COMMON HEADERS




---
### HTTP RESPONSE CODES

 - Informational responses (100–199)
 - Successful responses (200–299)
 - Redirects (300–399)
 - Client errors (400–499)
 - Server errors (500–599)


 - **2xx** Success
	- **200 OK**
	- **201 Created**
	- ...
 - **3xx** Redirect
	- **301** Moved Permanently
	- **302** Found
	- **304** Not modified
	- ...
 - **4xx** Client Error
	- 400 Bad Request
	- 401 Unauthorized
	- 403 Forbidden
	- 404 Not Found
	- 405 Method Not Allowed
	- ...
 - **5xx** Server Error
	- **500** Internal Server Error
	- **503**  Service Unavailable
	- **504** Gateway Timeout
	- ...

  ---
 ### HOW TO SEND HTTP REQUEST


 - BROWSER
	 - developer tools
	 - addons
 - APPLICATIONS
	 - Postman
	 - Insomnia
 - COMMAND LINE
	 - cUrl
	 - http-console
	 - HTTPie
 - IDE Plugins
	- WebStorm
	- VisualCode
 - CODE
	 - XMLHttpRequest
	 - Fetch
	 -  JQuery
	 - axios


