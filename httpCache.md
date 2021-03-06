# HTTP／1.1

## 通过ETag验证缓存的响应

- 服务器使用ETag HTTP标头传递验证令牌
- 验证令牌可实现高效的资源更新检查：资源未发生改变时不会传递任何数据

假定在首次获取资源 120 秒后，浏览器又对该资源发起了新的请求。首先，浏览器会检查本地缓存并找到之 前的响应。遗憾的是，该响应现已过期，浏览器无法使用。此时，浏览器可以直接发出新的请求并获取新的完 整响应。不过，这样做效率较低，因为如果资源未发生变化，那么下载与缓存中已有的完全相同的信息就毫无 道理可言！

客户端自动在"If-None-Match" HTTP 请求标头内提供 ETag 令牌。服务器根据当前资源核对令牌。如果 它未发生变化，服务器将返回"304 Not Modified"响应，告知浏览器缓存中的响应未发生变化，可以再延 用 120 秒。请注意，您不必再次下载响应，这节约了时间和带宽。

### Cache-Control

- 每个资源都可通过Cache-Control HTTP标头定义其缓存策略
- Cache-Control指令控制谁在什么条件下可以缓存响应以及可以缓存多久

从性能优化的角度来说，最佳请求是无需与服务器通信的请求：您可以通过响应的本地副本消除所有网络延迟 ，以及避免数据传送的流量费用。为实现此目的，HTTP 规范允许服务器返回 Cache-Control 指令，这些 指令控制浏览器和其他中间缓存如何缓存各个响应以及缓存多久。

Cache-Control 标头是在 HTTP/1.1 规范中定义的，取代了之前用来定义响应缓存策略的标头 （例如 Expires）。所有现代浏览器都支持 Cache-Control，因此，使用它就够了。

### "no-cache"和"no-store"

"no-cache"表示必须先与服务器确定返回响应是否发生了变化，然后才能用该响应来满足后续对同一网址的 请求。因此，如果存在合适的验证令牌 (ETag)，no-cache 会发起往返通信来验证缓存的响应，但如果资 源未发生变化，则可避免下载。

"no-store"则要简单得多。它直接禁止浏览器以及所有中间缓存存储任何版本的返回响应，例如，包含个人 隐私数据或银行业务数据的响应。每次用户请求该资源时，都会向服务器发送请求，并下载完整的响应。

### "public"与"private"

如果响应被标记为"public",则是可以被缓存的，即使有http验证和响应状态码没有正常的可缓存性，大多 数时候，public是不必要的，因为像"max-age"已经指明了资源的可缓存性。

相反，浏览器可以缓存私有响应，但是，这些响应通常用于单个用户，因此中间缓存不允许缓存它们。例如， 用户的浏览器可以缓存具有私有用户信息的HTML页面，但CDN无法缓存页面。

### "max-age"

此伪指令指定从请求时间允许重新使用获取的响应的最长时间（以秒为单位）。例如，"max-age = 60"表示 响应可以缓存并在接下来的60秒内重新使用。

# HTTP/1.0

## Last-Modified

在HTTP/1.0协议中，Last-Modified是控制缓存的一个非常重要的HTTP头。如果需要控制浏览器的缓存， 服务器首先必须发送一 个以UTC时间为值的Last-Modifeid头，当第二次访问这个页面时，浏览器会发送一 个If-Modified-Since头给服务器，让服务器 判断是否有必要更新内容，这个If-Modified-Since头的 值就是上次访问页面时，浏览器发送的Last-Modifeid头的值。

## Expires

Expires是HTTP/1.0中另外一个很重要的HTTP头，它表示缓存的存在时间，告诉客户端浏览器在这个时间之 前不对服务器发送请求，而直接使用浏览器的缓存。

## Pragma:no-cache

在HTTP/1.0中，可以使用Pragma: no-cache头来告诉浏览器不要缓存内容，它相当于HTTP/1.1中的Cache-Control:no-cache。
