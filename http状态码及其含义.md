> 1xx: 信息状态码

- 100 Continue：客户端应当继续发送请求。这个临时相应是用来通知客户端它的部分请求已经被服务器接受，且仍未被拒绝。客户端应当继续发送请求的剩余部分，或者如果请求已经完成，忽略这个响应。服务器必须在请求完成向客户端发送一个最终响应。
- 101 Switching Protocols：服务器已经理解了客户端的请求，并将通过Upgrade消息头通知客户端采用不同的协议来完成这个请求。在发送这个响应最后的空行后，服务器将会切换到Upgrade消息头中定义的那些协议。

> 2XX: 成状态码

- 200OK： 请求成功，请求所希望的响应头或数据体将随此响应返回
- 201 Created：
- 202 Accepted
- 203 Non-Authoritative Information
- 204 No Content: 请求成功，但没有资源可返回，浏览器不会发生更新，一般在只需要从客户端往服务器端发送消息，而对客户端不需要发送新消息内容的情况下使用
- 205 Reset Content
- 206 Partial Content：表示客户端进行了范围请求，而服务器成功执行了这部分的GET请求。响应报文包含由Content-Range指定范围的实体内容

> 3XX: 重定向，表明浏览器需要执行某些特殊的处理以正确处理请求

- 300 Multiple Choices:
- 301 Moved Permanently：永久性重定向。表示请求的资源已被分配了新的URI，以后应使用现在所指的URI
- 302 Found：临时性重定向。表示请求的资源已经被分配了新的URI，希望用户（本次）能使用新的URI访问
- 303 See Other：该状态码表示由于请求对应的资源存在着另外一个URI，应使用GET方法定向获取请求的资源
- 304 Not Modified：该状态码表示客户端发送附带条件的请求时，服务器端允许请求访问资源，但未满足条件的情况。304状态码返回时，不包含任何响应的主体部分
- 307 Temporary Redirect：临时重定向，该状态码与302Found有着相同的含义，尽管302标准禁止POST变换成GET，但实际使用时大家并不遵守。307会遵守浏览器的标准，不会从POST编程GET

> 4XX 客户端错误

- 400 Bad Request：该状态码表示请求报文中存在语法错误。当错误发生时，需修改请求的内容再次发送请求。另外浏览器会像200 OK一样对待该状态码
- 402 Payment Required：
- 401 Unauthorized：该状态码表示发送的请求需要有通过HTTP认证的认证信息，另外若之前已进行过1此请求，则表示用户认证失败
- 403 Forbidden：该状态码表明对请求资源的访问被服务器拒绝了。服务器端没有必要给出详细理由
- 404 NotFound： 服务器上无法找到请求的资源
- 405 Method Not Allowed
- 406 Not Acceptable
- 407: Proxy Authentication Required

> 5XX服务器错误

- 500 Internal Server Error：该状态码表明服务器端在执行请求时发生了错误，也可能是Web应用存在的bug或某些临时的故障
- 502 Bad Gateway
- 503 Service Unavailable：该状态码表明服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。 - -
