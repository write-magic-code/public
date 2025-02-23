# CORS

刚学一点前后端的学生，碰到的第一个大坑估计就是 CORS，写毕设的时候被这个折磨几天很要命。

## 具体表现

如果你看到了形如这种报错：

```
Access to fetch at 'https://aaa.com/' from origin 'https://bbb.com' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.
```

恭喜你，来对地方了。这里的重点是：**has been blocked by CORS policy**，搞清楚 CORS policy，这个问题就能解决。

## 先讲概念

了解 CORS 的设计、行为及其解决方案之前，需要先对 CORS 解决的问题有基本认知。

1. 一个 HTTP 请求中服务端返回的数据称为**资源**，资源可以是 HTML 页面、图片、视频、CSS、JS 等。
2. 服务器的标识称为**源**，源由协议、域名、端口号组成，比如 `https://www.example.com:443`。

比如说你前端起的开发服务器（比如 http://localhost:5173）是一个源，后端起的服务（比如 https://api.github.com）是另一个源。而 CORS，全名「Cross-Origin Resource Sharing」，就是用于管理在不同的源下获取资源的策略。

## 跨域是危险的

假设你登陆了网站 A，你在 A 的网页里向服务端获取资源都是带着 Cookie 的，Cookie 里会带着当前登陆的用户的信息，这样服务端才能知道这是你。但是现在有一个问题，如果你打开的网站是 B，但是请求的源是 A，那 A 的 Cookie 到底要不要传给 A？

- 如果传：你是说 B 上的代码可以带着 A 上的登陆信息随便跟 A 交互？那 B 改我密码怎么办？
- 如果不传：如果 B 真的需要 A 的数据，你不给的话怎么办？

因此需要制定一种规范，让一个源可以在知情的情况下，授权（或者部分授权）另一个源的访问，这个规范就是 CORS。

## CORS

CORS 是一种基于 HTTP 头的机制，通过特定的头让浏览器和服务器进行沟通，从而决定是否允许一个源的网页访问另一个源的服务器上的资源。

CORS 对于简单请求和复杂请求有两套不同的方案，简单请求是浏览器认为对服务器影响较小，且安全的操作，比如使用 GET 请求方法、没有自定义的请求头而且传输的文件格式是纯文本，浏览器就会认为这只是向服务器获取资源，无论如何请求都不会有严重的影响（比如意外修改、删除数据），反之则是复杂请求。

对于简单请求，浏览器会自动往请求里塞 `Origin` 这个 header，其值是当前的源（协议、域名和端口），如果此时服务端返回的响应中的 `Access-Control-Allow-Origin` 中没有包含源 A 的源，则浏览器会自动报错，虽然 HTTP 请求是确实发生并且完成了，但是源 A 并不会得到这次源 B 返回的数据。

对于复杂请求，浏览器会先发送一个预检请求（preflight），带上当前的 `Origin` 向服务端询问是否允许跨域访问。在预检请求中，服务端需要返回 `Access-Control-Allow-Origin` 来明确允许的跨域源，并返回 `Access-Control-Allow-Methods` 来指定允许的 HTTP 方法。此外，服务端还可以根据需要返回 `Access-Control-Allow-Headers` 来允许自定义请求头，以及 `Access-Control-Allow-Credentials` 来决定是否允许携带凭证（如 Cookie）。如果预检通过，浏览器会继续执行原始请求，之后的流程与简单请求一致。

如果没有预检机制，即使浏览器报错，对服务端有影响的请求仍被发送，导致安全问题，例如「在网站 A 点击后意外修改了网站 B 的密码」。

基于这套机制，CORS 作为浏览器的安全策略，能防止大部分的跨域恶意攻击。除非一些 API 的设计实在跟 Web 标准相去甚远，否则不会有太多问题。

## 前端的权宜之计

严格来说，CORS 报错只能通过后端添加 HTTP 请求头才能解决，但是在开发过程中，前端也有一些权宜之计可以**暂时绕过**这个问题。

### 使用代理

Webpack 和 Vite 都提供了内置的 proxy 功能，它的原理是设置一个代理服务器，前端请求代理服务器，代理服务器再请求真正的服务器，只要代理服务器跟前端在同一个源下，就不会有跨域问题。

对于 Webpack 用户，可以参考[中文文档（非官方但是有官方背书）](https://webpack.docschina.org/configuration/dev-server/#devserverproxy)。对于 Vite 用户，可以参考[官方中文文档](https://cn.vite.dev/config/server-options#server-proxy)。

### 在服务端获取数据

如果你使用 Next.js 这种带有服务端的前端框架，那么可以考虑在服务端获取数据，比如在 App router 中：

```jsx
export default async function Page() {
    const data = await fetch('https://api.github.com').then(res => res.json());
}
```

只要当前页面永远在服务端渲染（这意味着你没法 `'use-client'`），那么就不会有跨域问题。但是现实情况是，有大量的请求是在客户端发起的，比如用户点击按钮、滚动页面等等。

或者你也可以使用 Next.js 的 API 路由，把数据请求放在 API 路由中，然后在页面中请求 API 路由，这几乎能应付大部分情况，但是这意味着你重新实现了后端的访问控制，造成了系统间**权责不清**和**重复工作**。

## 后端的解决方案

大部分的现代后端框架都有提供配置 CORS 策略的功能，再不济也会有官方文档告诉你怎么配置。在搜索引擎中搜索「CORS + 你使用的框架」即可。比如对于 Gin 框架：

```go
package main

import (
  "time"

  "github.com/gin-contrib/cors"
  "github.com/gin-gonic/gin"
)

func main() {
  router := gin.Default()
  router.Use(cors.New(cors.Config{
    AllowOrigins:     []string{"https://foo.com"},
    AllowMethods:     []string{"PUT", "PATCH"},
    AllowHeaders:     []string{"Origin"},
    ExposeHeaders:    []string{"Content-Length"},
    AllowCredentials: true,
    AllowOriginFunc: func(origin string) bool {
      return origin == "https://github.com"
    },
    MaxAge: 12 * time.Hour,
  }))
  router.Run()
}
```

具体的实现细节可能会跟框架的实现有关，但是大意是一致的。

重要的事情说三遍：**一定要在开发服务器上提前测试 CORS！一定要在开发服务器上提前测试 CORS！一定要在开发服务器上提前测试 CORS！** 别等上线了，前端一堆报错，你才手忙脚乱地改配置。

如果项目使用 Nginx 或 Caddy 之类的反向代理服务器，那么也可以在这些服务器上加头来配置 CORS 策略，这样就不用在每个后端服务上都配置一遍，在某些时候会更加方便。

