## [HTTP/2 Frames](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages#http2_frames "Permalink to HTTP/2 Frames")

HTTP/1.x messages have a few drawbacks for performance:

-   Headers, unlike bodies, are uncompressed.
-   Headers are often very similar from one message to the next one, yet still repeated across connections.
-   No multiplexing can be done. Several connections need opening on the same server: and warm TCP connections are more efficient than cold ones.

HTTP/2 introduces an extra step: it divides HTTP/1.x messages into frames which are embedded in a stream. Data and header frames are separated, which allows header compression. Several streams can be combined together, a process called _multiplexing_, allowing more efficient use of underlying TCP connections.

![HTTP/2 modify the HTTP message to divide them in frames (part of a single stream), allowing for more optimization.](https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages/binary_framing2.png)

HTTP frames are now transparent to Web developers. This is an additional step in HTTP/2, between HTTP/1.1 messages and the underlying transport protocol. No changes are needed in the APIs used by Web developers to utilize HTTP frames; when available in both the browser and the server, HTTP/2 is switched on and used.
