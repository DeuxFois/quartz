# HTTP Messages

HTTP messages are how data is exchanged between a server and a client. There are two types of messages: [HTTP Request](Web&Net/Net/HTTP%20Request.md) sent by the client to trigger an action on the server, and [HTPP Response](Web&Net/Net/HTPP%20Response.md), the answer from the server.

Web developers, or webmasters, rarely craft these textual HTTP messages themselves: software, a Web browser, proxy, or Web server, perform this action. They provide HTTP messages through config files (for proxies or servers), APIs (for browsers), or other interfaces.

**The 2 main point are :**  
[Methods](Web&Net/Net/Methods.md)  
[Status Codes](Web&Net/Net/Status%20Codes.md)

See also :  
[HTTP 2](Web&Net/Net/HTTP%202.md)  
[HTTP3](Web&Net/Net/HTTP3.md)  

![From a user-, script-, or server- generated event, an HTTP/1.x msg is generated, and if HTTP/2 is in use, it is binary framed into an HTTP/2 stream, then sent.](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/httpmsg2.png)


![Requests and responses share a common structure in HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/httpmsgstructure2.png)

## Conclusion

HTTP messages are the key in using HTTP; their structure is simple, and they are highly extensible. The HTTP/2 framing mechanism adds a new intermediate layer between the HTTP/1.x syntax and the underlying transport protocol, without fundamentally modifying it: building upon proven mechanisms.