
## [HTTP Requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#http_requests "Permalink to HTTP Requests")

### [Start line](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#start_line "Permalink to Start line")

HTTP requests are messages sent by the client to initiate an action on the server. Their _start-line_ contain three elements:

1.  An [HTTP Methods](Web-Network/Network/Methods.md),
2.  The _request target_, usually a URL , or the absolute path of the protocol, port, and domain are usually characterized by the request context. The format of this request target varies between different HTTP methods. It can be
    -   An absolute path, ultimately followed by a `'?'` and query string. This is the most common form, known as the _origin form_, and is used with `GET`, `POST`, `HEAD`, and `OPTIONS` methods.
        -   `POST / HTTP/1.1`
        -   `GET /background.png HTTP/1.0`
        -   `HEAD /test.html?query=alibaba HTTP/1.1`
        -   `OPTIONS /anypage.html HTTP/1.0`
    -   A complete URL, known as the _absolute form_, is mostly used with `GET` when connected to a proxy. `GET https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages HTTP/1.1`
    -   The authority component of a URL, consisting of the domain name and optionally the port (prefixed by a `':'`), is called the _authority form_. It is only used with `CONNECT` when setting up an HTTP tunnel. `CONNECT developer.mozilla.org:80 HTTP/1.1`
    -   The _asterisk form_, a simple asterisk (`'*'`) is used with `OPTIONS`, representing the server as a whole. `OPTIONS * HTTP/1.1`
3.  The _HTTP version_, which defines the structure of the remaining message, acting as an indicator of the expected version to use for the response.

### [Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#headers "Permalink to Headers")

[HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) from a request follow the same basic structure of an HTTP header: a case-insensitive string followed by a colon (`':'`) and a value whose structure depends upon the header. The whole header, including the value, consist of one single line, which can be quite long.

Many different headers can appear in requests. They can be divided in several groups:

-   [General headers](https://developer.mozilla.org/en-US/docs/Glossary/General_header), like [`Via`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Via), apply to the message as a whole.
-   [Request headers](https://developer.mozilla.org/en-US/docs/Glossary/Request_header), like [`User-Agent`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent) or [`Accept`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept), modify the request by specifying it further (like [`Accept-Language`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Language)), by giving context (like [`Referer`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer)), or by conditionally restricting it (like [`If-None`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-None "This is a link to an unwritten page")).
-   [Representation headers](https://developer.mozilla.org/en-US/docs/Glossary/Representation_header) like [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) that describe the original format of the message data and any encoding applied (only present if the message has a body).

![Example of headers in an HTTP request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/http_request_headers3.png)

### [Body](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#body "Permalink to Body")

The final part of the request is its body. Not all requests have one: requests fetching resources, like `GET`, `HEAD`, `DELETE`, or `OPTIONS`, usually don't need one. Some requests send data to the server in order to update it: as often the case with `POST` requests (containing HTML form data).

Bodies can be broadly divided into two categories:

-   Single-resource bodies, consisting of one single file, defined by the two headers: [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) and [`Content-Length`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Length).
-   [Multiple-resource bodies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types#multipartform-data), consisting of a multipart body, each containing a different bit of information. This is typically associated with [HTML Forms](https://developer.mozilla.org/en-US/docs/Learn/Forms).

