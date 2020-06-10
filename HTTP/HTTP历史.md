HTTP是万维网的基础协议。早起是一个实验室之间交换文件的协议，进化到如今可以传输图片，高分辨率视频、3D效果的复杂的现代互联网协议。

## 万维网

1989年，Tim Berners-Lee 博士写了一份关于建立一个通过网络传输超文本协议的系统报告。起初命名为Mesh，在1990年项目实施期间被更名为万维网（World Wide Web）。

万维网在现有的TCP/IP协议基础上建立，由四部分组成：

- 用来表示超文本文档的文本格式，超文本标记语言（HTML）
- 用来交换超文本文档的简单协议，超文本传输协议（HTTP）
- 显示/编辑超文本文档的客户端，即网络浏览器。
- 服务器用于提供访问的文档，即`httpd`的前身

HTTP在应用的早起阶段非常的简单，后来被称为HTTP/0.9，也有叫做单行（one-line）协议

## HTTP/0.9-单行协议

请求由单行指令构成，以唯一可用的`Get`开头，其后跟着目标资源的路径

``` http
GET /index.html
```

响应也极其简单：只包含文档本身，没有响应头，这意味着只有HTML文件可以传送

``` html
<html>
  这是一个简单的HTML页面
</html>
```

- 只有GET命令
- 没有HEADER等描述数据的信息
- 服务器发送完毕，就关闭TCP连接
- 一旦出现问题，一个特殊的包含问题描述信息的HTML文件将被发回，供人们查看

## HTTP/1.0-构建可扩展性

- 协议版本信息会随着每个请求发送 `GET /index.html HTTP/1.0`
- 状态码会在响应开始时发送，使浏览器能了解请求执行成功或失败，并相应调整行为（如更新或使用本地缓存）
- 引入了HTTP头的概念，无论是对于请求还是响应，允许传输元数据，使协议变得非常灵活，更具扩展性
- 具备传输纯文本HTML文档以外的其他类型文档的能力（`Content-Type`）
- 增加了很多命令
- 增加status code 和 header
- 多字符集支持、多部分发送、权限、缓存

``` http
GET /index.html HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)

200 OK
Date: Tue, 15 Nov 1994 08:12:31 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/html
<HTML> 
一个包含图片的页面
</HTML>
```

这些新扩展并没有马上被引入到标准中。直到1996年11月，一份新的文档被发表出来。该文档定义了HTTP/1.0，但它是狭义的，并不是官方标准。 

## HTTP/1.1-标准化的协议

在1997年，[HTTP/1.1](https://tools.ietf.org/html/rfc2068)标准发布，就在HTTP/1.0发布的几个月后。

HTTP /1.1消除了大量歧义内容并引入了更多改进：

- 连接可复用，节省多次打开TCP连接加载文档资源的时间
- 增加管线技术pipeline，第一个应答被完全发送之前就发送第二个请求，降低通信延迟(同一个连接里发送多个请求，<u>串行</u>)
- 支持响应分块
- 缓存控制机制
- 内容协商机制
- 增加host和其他一些命令（同一个IP可以部署多个web服务）

## HTTP/2.0-为了更优异的表现

2015年5月正式标准化

> HTTP/2.0是二进制协议，不是文本协议

- 所有数据以二进制传输（之前是字符串）
- 同一个连接里发送多个请求可<u>并行</u>处理，移除HTTP/1.x中的顺序和阻塞约束
- 头信息压缩，移除重复和传输重复数据的成本
- 推送

## HTTP-安全传输

1994年，网景公司在TCP/IP协议栈基础上创建了一个**额外的加密传输层**：SSL。SSL2.0及其后继者SSL 3.0和SSL 3.1**允许通过加密来保证服务器和客户端之间交换消息的真实性**。SSL标准化最终成为TLS。

## HTTP-复杂应用

Web 的最初设想不是一个只读媒体。 他设想一个 Web 是可以远程添加或移动文档，是一种分布式文件系统。 大约 1996 年，HTTP 被扩展到允许创作，并且创建了一个名为 WebDAV 的标准。

在 2000 年，一种新的使用 HTTP 的模式被设计出来：[representational state transfer](https://developer.mozilla.org/en-US/docs/Glossary/REST) (或者说 `REST`)。 <u>由 API 发起的操作不再通过新的 HTTP 方法传达，而只能通过使用基本的 HTTP / 1.1 方法访问特定的 URI</u>。 这允许任何 Web 应用程序通过提供 API 以允许查看和修改其数据，而无需更新浏览器或服务器

其中几个API为特定目的扩张了HTT协议，大部分是新的特定HTTP头

- [Server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)，服务器可以偶尔推送消息到浏览器。
- [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket_API)，一个新协议，可以通过升级现有 HTTP 协议来建立。

## 放松的Web安全模型

HTTP和Web安全模型--[同源策略](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)是互不相关的。事实上，当前的Web安全模型是在HTTP被创造出来后才被发展的。

