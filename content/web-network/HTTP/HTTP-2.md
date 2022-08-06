# The beginning oh HTTP/2

HTTP/2 has made some serious improvements with 
non-blocking downloads, pipelines and push servers that helped us overcome some of the limitations of the underlying TCP protocol. This allowed us to minimize the number of request-response cycles.

HTTP/2 allowed us to push more than one resource into a single TCP connection - multiplexing. We also got more flexibility in the order of static downloads, and our pages are no longer limited by a linear progression of downloads.

Translated with www.DeepL.com/Translator (free version)

![HTTP/2 push](https://kinsta.com/fr/wp-content/uploads/sites/4/2016/04/push-http2.png)

Push HTTP/2

In practice, this means that one important javascript resource is not necessarily a choke point for all the other static resources waiting for their turn.

![No pipelining vs pipelining](https://kinsta.com/fr/wp-content/uploads/sites/4/2019/03/pas-pipelining-pipelining.png)

No pipelining vs pipelining 

Add to this the HPACK compression of the HTTP/2 header and the default binary format of the data transfer, and we have, in many cases, a much more efficient protocol.

![HTTP/2 HPACK Compression](https://kinsta.com/fr/wp-content/uploads/sites/4/2016/04/compression-http2-hpack.png)

Compression HTTP/2 HPACK

Major browser implementations made it necessary to implement encryption – SSL – in order to take advantage of the benefits of HTTP/2 – and sometimes this resulted in a computational overhead that made the speed improvements unnoticeable. There have even been instances where users have reported a slowdown after transitioning to HTTP/2.

Let’s just say that the early days of adopting this build weren’t for the faint of heart.

The Nginx implementation also lacked the server push feature, relying on a module. And [Nginx modules aren’t the](https://kinsta.com/fr/blog/nginx-vs-apache/) usual Apache modules you can just copy – NGINX needs to be recompiled with those.

Although some of these issues are now resolved, if we look at the entire protocol stack, we see that the main constraint is at a lower level than HTTP/2 dared to attempt.

To elaborate on this, we will dissect today’s Internet Protocol stack from its bottom layer upwards. If you want to know more about the background of HTTP/2, feel free to check out our [ultimate HTTP/2 guide](https://kinsta.com/fr/apprendre/http2/) .