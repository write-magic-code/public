## 1. HTTP 意味着什么

HTTP 协议是当前最常用的通信协议，大部分互联网公司的产品都通过网页或者 App （统称应用）向用户呈现，应用与服务器的通信一般由 HTTP 及其变体（比如 WebSocket）进行。

## 一般的 HTTP 报文

```http
POST /cgi-bin/process.cgi HTTP/1.1
User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)
Host: www.tutorialspoint.com
Content-Type: application/x-www-form-urlencoded
Content-Length: length
Accept-Language: en-us
Accept-Encoding: gzip, deflate
Connection: Keep-Alive

licenseID=string&content=string&/paramsXML=string
```

<!--

HTTP 协议结构：
1. HTTP Header
2. HTTP Body

Request 报文结构：
1. HTTP方法 请求路径 HTTP版本 换行
2. HTTP Header 是一个键值对（key-value pair），key 是 http header 的各种属性名，value是属性对应的内容，以英文冒号（:）分割，换行结束一对
   1. 其中`Content-Type`描述body中内容的类型，让后端程序可以正确的解析后面的内容
   2. 其中``
   3.
-->

上面的部分中：
1. `POST` 是协议的 method，一般来说在规范中，"GET" 是查询，"POST" 是新增，"DELETE" 是删除等等，但是 method 只是规范（一种行业倡议），跟具体的实现无关，你仍然可以在 GET 中删除某物。
2. 下面的几行，`User-Agent` 到 `Connection` 都是请求的 Header，经常被用于做一些验证，比如把传输的内容格式放在 `Content-Type` header 里让后端程序可以正确的解析后面的内容。
3. 最后的一行，是 body，这里是正式传输内容的部分，内容格式可以是任何东西，比如 json/xml 或者一些转换过的二进制文件。

```http
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache/2.2.14 (Win32)
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
Content-Length: 88
Content-Type: text/html
Connection: Closed

{ "id": 123" }
```

响应与上面的请求类似，不过第一行内容不一样，又一个陌生的：200 OK。

这是请求的 Code，标识这次请求是否成功，数字称为状态码，规则如下：

1. 101 是切换协议
2. 20_ 请求正常
3. 30_ 后端请求前端跳转到新页面
4. 40_ 是后端认为前端的输入有误
5. 50_ 是后端死了