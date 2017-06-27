# 电影爬虫（一）

> 虽然名字叫电影爬虫，其实本期内容和电影没什么关系。想必你注意到了，本文只是第一篇，主要是为后面的打基础。为了能够爬到你想看电影的种子和磁力链接，大家就耐心地看一下咯。

## 请求方式

从资源的角度来看，网络中的一切数据都是资源，包括但不限于：文本、图片、音频、视频。爬虫的本质就是要从通过计算机网络，获取这些资源。

爬虫是基于HTTP协议工作的，HTTP协议有以下四种基本的资源请求方法。

| http请求方式 | 说明                                   |
| -------- | ------------------------------------ |
| GET      | 向特定的资源发出请求                           |
| POST     | 向特定的资源发出请求并提交数据，可能会导致新的资源的创建和已有资源的修改 |
| PUT      | 向指定资源位置上上传最新的内容                      |
| DELETE   | 删除指定的资源                              |

除了上面的四种，还有HEAD, CONNECT, OPTIONS等方法[[1]](http://www.cnblogs.com/liangxiaofeng/p/5798607.html)。

以上的HTTP方法中，只有GET和POST涉及到获取资源，因此，GET和POST方法是爬虫用的最多的请求方法。

## 状态码

在HTTP请求的时候，总会出现各种各样的问题，人们就用状态码来指代这些问题。常见的状态码有：

| 状态码  | 含义                                       |
| ---- | ---------------------------------------- |
| 200  | 服务器已经成功处理了请求                             |
| 300  | 针对请求，服务器可执行多种操作。 服务器或请求者需要作出选择           |
| 301  | 请求的网页已经永久移动到新位置，服务器会自动重定向到指定的位置          |
| 401  | 未授权，服务器需要身份验证，一般来说需要登陆                   |
| 403  | 服务器拒绝请求，可能的原因有很多，请参考文章[http://www.mahaixiang.cn/seoyjy/403.html](http://www.mahaixiang.cn/seoyjy/403.html)[2] |
| 404  | 服务器找不到请求的网页                              |
| 500  | 服务器内部错误，不能正确响应请求                         |
| 504  | 网关超时，网关或者代理未能及时从上游服务器接受到响应               |

虽然状态码有很多种，但还是有规律可循的：

| 范围      | 含义                    |
| ------- | --------------------- |
| 100~199 | 服务器已收到请求，需要客户端完成更多的动作 |
| 200~299 | 请求成功，服务器已接受请求并处理      |
| 300~399 | 未得到请求的资源，需要重定向后获取     |
| 400~499 | 客户端错误，请求有语法错误         |
| 500~599 | 服务器错误                 |

更多详细状态码，请参考《HTTP状态码详解》[3]：http://tool.oschina.net/commons?type=5。

根据返回状态码，我们可以得知HTTP请求的具体情况，然后对爬虫代码作出适当的调整。

## requests库

requests库可以实现HTTP请求[[4]](http://www.python-requests.org/en/master/)。只要指定URL和请求方法，就能够获得服务器的响应内容，这些响应的内容中就有我们想要的网页信息。

requests对象有很多属性：status_code代表状态码，encoding代表网页编码，content代表二进制流，text代表文本信息。requests库会自动解码，也可以指定编码。有时，调用text属性返回的内容乱码，这时就我们需要指定合适的编码使内容正确地显示。

```python
>>> import requests
# 注意：url必须包含协议名称 "http://"
>>> url = 'http://www.baidu.com'
>>> r = requests.get(url)
# 获取状态码，200表示服务已经正确处理了请求
>>> r.status_code
200
>>> r.json
<bound method Response.json of <Response [200]>>
# 指定网页编码为utf-8
>>> r.encoding = 'utf-8'
# 获取网页内容，二进制流，数据类型为bytes
>>> r.content
# 获取网页内容，文本信息，数据类型是Unicode
>>> r.text
```

## 总结

1. 根据需要，使用不同的HTTP请求获取服务器上的资源
2. 每一次HTTP请求都有一个状态码，根据状态码，我们可以知道HTTP请求的情况
3. requests库可以实现HTTP请求

## 参考资料

1. 《HTTP协议的8种请求类型介绍》，原文链接：http://www.cnblogs.com/liangxiaofeng/p/5798607.html
2. 《403 Forbidden错误的原因和解决方法》，原文链接：http://www.mahaixiang.cn/seoyjy/403.html
3. 《HTTP状态码详解》，原文链接：http://tool.oschina.net/commons?type=5
4. requests官方文档《Requests: HTTP for Humans》，链接：http://www.python-requests.org/en/master/