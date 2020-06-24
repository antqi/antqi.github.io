## 为什么要HTTP缓存？-[MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ)

客户端发送HTTP请求到服务端响应成功，中间需要建立TCP连接，三次握手，再是通信数据，再是四次挥手结束响应。这个过程需要的多次的往返通信，拖延了浏览器处理数据的时间，增加了访问者和服务器的流量成本，请求或数据过多时也降低了浏览体验。

而缓存直白一些说

- 可以节省用户的流量
- 减少服务器资源损耗

那如何缓存呢？

## 如何缓存-缓存控制

这里讲讲如何从HTTP头部字段来缓存，主要`Expires` 和 `Cache-Control`两个头部字段。

Expires是HTTP1.0的时候存在的字段，Cache-Control 则是HTTP/1.1定义。都是请求响应通用头部，但一些指令值有不一样。

- Expires:缓存过期时间

  ``` http
  Expires:<http-date>
  ```

- Cache-Control [有多项值](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control)，这里主要说明一些常见的指令值

  - no-store & no-cache

  - public & private

  - max-age

    

### Expires (HTTP/1.0 +)

服务器在HTTP 响应头部添加`Expires:<http-date>`字段，对象的字段值是缓存过期时间，如：

``` HTTP
Expires: Thu Jun 11 2020 15:35:38 GMT+0800 
```

浏览器在解析到HTTP响应头中的`Expires`时，会先将该资源缓存起来。等到下一次再次访问这个资源的时候，浏览器会先检查缓存：<u>*当前时间*是否超过`Expires`</u>，如果没有超过，浏览器不会发送该资源的请求，直接从缓存中读取资源（如图：form disk cache）。

![image-20200611155640808](/Users/ant_qi/Library/Application Support/typora-user-images/image-20200611155640808.png)

但有个小问题：浏览器所在的设备是用户设备，这个当前时间是不可靠的。如果用户设置电脑的时间是3000年，那么浏览器得到的‘当前时间’则是3000，岂不是表示缓存永远过期了，那么这个缓存是无意义的？



### Cache-Control（HTTP/1.1+）

HTTP/1.1 提供了一个<u>请求和响应的通用消息头字段</u> `Cache-Control` ,`Cache-Control`可以解决使用 `Expires`时由用户自动修改本地时间导致的缓存失效问题，并提供了更灵活的缓存机制。

Cache-Control提供了[多项属性值](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control)，来达到灵活控制缓存。

### max-age=<seconds>

而其中的`Cache-Control: max-age=<seconds>`可以解决上述的Expires问题。给响应头设置`Cache-Control: max-age=30` 表示服务器告诉浏览器该资源在30秒之后过期。

如果浏览器在30秒内再次请求该资源，则服务器不会返回内容，并且浏览器读取的是缓存。

![image-20200611172530382](/Users/ant_qi/Library/Application Support/typora-user-images/image-20200611172530382.png)

如果浏览器在30秒后再词请求资源，则会使用服务器返回的内容，并将新的内容缓存。

![image-20200611172742569](/Users/ant_qi/Library/Application Support/typora-user-images/image-20200611172742569.png)

**当`Cache-Control: max-age=<seconds>`和`Expires`同时设置时，以`Cache-Control: max-age=<seconds>`为准**。[参考](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9.3)



### public & private

public，响应可以被任何对象（发起请求的客户端，代理服务器等）缓存。即使是不可缓存的内容（没有设置max-age、expires等，或者是post请求）。

private， 响应只能被单个用户缓存，不能作为共享缓存。私有缓存可以响应内容，比如用户发送请求的浏览器。



### no-store & no-cache

no-store，没有缓存。

禁止浏览器缓存任何请求及响应的内容。每次客户端请求资源时，都会向服务器发送请求，并下载完整服务器响应的内容。

``` http
Cache-Control: no-store
```

no-cache，缓存但重新验证。

表示必须先与服务器验证资源是否过期，更具响应结果来决定是否用缓存。若过期返回新的内容，未过期返回 HTTP code 304，告诉浏览器为过期使用本地缓存，避免重新下载资源。

``` http
Cache-Control: no-cache
```

**强调一点：no-store表示永远不用缓存，no-cache 每次先检查再看是否使用缓存**

<u>那如何验证缓存是否过期呢？</u>



## 缓存过期了-验证缓存

通过HTTP头部验证缓存有两种方式：

- 响应头部`Last-Modified` & 请求头部 `If-Modified-Since`
- 响应头部`Etag` & 请求头部`If-None-Match`



#### Last-Modified & If-Modified-Since|If-Unmodified-Since

##### 如何起作用

我们先看下组合 `Last-Modified & If-Modified-Since`。[If-Unmodified-Since](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-Unmodified-Since)与`If-Modified-Since`用法一样，意义相反，所以这里只说`If-Modified-Since`

该组合验证请求资源是否编辑过，如果编辑过则响应304，未编辑则是200。

##### If-Modified-Since是什么

请求头部 [If-Modified-Since](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-Modified-Since)，资源最后一次修改时间。需要注意的是`If-Modified-Since`只可以用在 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 或 [`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD) 请求中

```http
If-Modified-Since: <day-name>, <day> <month> <year> <hour>:<minute>:<second> GMT
```

##### Last-Modified是什么

响应头部[Last-Modified](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Last-Modified)，表示资源的最后一次修改时间

```http
Last-Modified: <day-name>, <day> <month> <year> <hour>:<minute>:<second> GMT
```

**第一次请求响应**

Response

``` http
cache-control: max-age=3600
content-type: image/webp
date: Tue, 16 Jun 2020 02:38:20 GMT
expires: Tue, 16 Jun 2020 03:38:20 GMT
last-modified: Tue, 17 Mar 2020 17:03:38 GMT
```

**第二次请求响应**

请求体中会带有`If-modified-since`  询问服务器该资源是否有编辑

Request

``` http
if-modified-since:Tue, 17 Mar 2020 18:31:38 GMT
```

Response

```http
Status Code: 304 Not Modified
...
last-modified: Tue, 17 Mar 2020 17:03:38 GMT 
```

Last-Modified & If-Modified-Since询问的是资源有没有编辑，如果文件直至打开，键入几个字再删除这几个字，再保存呢，这也是编辑了，但其实内容是无变化的。问题：

<u>如何知道服务器文件内容没有改动？</u>

#### Etag & If-None-Match

##### Etag是什么？

Etag是HTTP响应头的资源的特定版本的标识符。当设置了该头部：

- 如果内容没有改变，Web服务器不需要发送完整的内容
- 如果内容有修改，Etag则会更新，可以理解为文件的MD5值

##### Etag语法

```
ETag: W/"<etag_value>"
ETag: "<etag_value>"
```

- W/  

  可选表示大小写敏感

- <etag_value> 

  没有明确指定生成ETag值的方法，可HASH，可MD5 ，可自定义的版本

Etag只是规定了响应的内容唯一标识，而需要真正起到作用，还需要与请求头部 `If-None-Match`配合使用

##### If-None-Match是什么

If-None-Match是一个条件式请求头部。

- 对于 GET[`GET`](https://wiki.developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 和 [`HEAD`](https://wiki.developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD) 请求方法：
  - 当服务器没有任何的资源Etag与请求头Etag值相同时候，返回所请求的资源，状态码为200
  - 而验证失败时，服务器返回304 Not Modified状态码，并且同时生成对应的200 响应中的首部：Cache-Control、Content-Location、Date、ETag、Expires 和 Vary 。
- 对于其他方法：
  - 当服务器没有任何资源的Etag属性值与请求的Etag属性值相匹配时，才会对请求做出相应处理。
  - 验证失败时，响应状态码` 412 Precondition Failed`（先决条件失败）表示客户端错误

##### If-None-Match语法

```
If-None-Match: <etag_value>
If-None-Match: <etag_value>, <etag_value>, …
If-None-Match: *
```

- <etag_value>

  形式是采用双引号括起来的由 ASCII 字符串（如"675af34563dc-tr34"）

- *

  可以代表任意资源，只在用在资源上传时，一般用PUT方法，来·检测拥有相同识别ID的资源是否已经上传过了

  

#### If-Modified-Since & If-None-Match 

**<u>当与If-Modified-Since一同使用的时候，If-None-Match 优先级更高</u>**



#### 小总结：

> no-cache 是开启缓存校验的钥匙

设置了过期时间而没有设置no-cache ，在max-age 有效期内，浏览器不会发送该资源请求，直接获取本地缓存

若设置了过期时间，且设置了no-cache ,会发送请求，并做缓存校验