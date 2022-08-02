## [HTTP Responses](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#http_responses "Permalink to HTTP Responses")

### [Status line](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#status_line "Permalink to Status line")

The start line of an HTTP response, called the _status line_, contains the following information:

1.  The _protocol version_, usually `HTTP/1.1`.
2.  A [Status Codes](Web&Net/Net/Status%20Codes.md), indicating success or failure of the request. Common status codes are [`200`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/200), [`404`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404), or [`302`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302)
3.  A _status text_. A brief, purely informational, textual description of the status code to help a human understand the HTTP message.

A typical status line looks like: `HTTP/1.1 404 Not Found`.

### [Headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#headers_2 "Permalink to Headers")

[HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) for responses follow the same structure as any other header: a case-insensitive string followed by a colon (`':'`) and a value whose structure depends upon the type of the header. The whole header, including its value, presents as a single line.

Many different headers can appear in responses. These can be divided into several groups:

-   [General headers](https://developer.mozilla.org/en-US/docs/Glossary/General_header), like [`Via`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Via), apply to the whole message.
-   [Response headers](https://developer.mozilla.org/en-US/docs/Glossary/Response_header), like [`Vary`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Vary) and [`Accept-Ranges`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept-Ranges), give additional information about the server which doesn't fit in the status line.
-   [Representation headers](https://developer.mozilla.org/en-US/docs/Glossary/Representation_header) like [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) that describe the original format of the message data and any encoding applied (only present if the message has a body).

![Example of headers in an HTTP response](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/http_response_headers3.png)

### [Body](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#body_2 "Permalink to Body")

The last part of a response is the body. Not all responses have one: responses with a status code that sufficiently answers the request without the need for corresponding payload (like [`201`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/201) **`Created`** or [`204`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/204) **`No Content`**) usually don't.

Bodies can be broadly divided into three categories:

-   Single-resource bodies, consisting of a single file of known length, defined by the two headers: [`Content-Type`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Type) and [`Content-Length`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Length).
-   Single-resource bodies, consisting of a single file of unknown length, encoded by chunks with [`Transfer-Encoding`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Transfer-Encoding) set to `chunked`.
-   [Multiple-resource bodies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types#multipartform-data), consisting of a multipart body, each containing a different section of information. These are relatively rare.

