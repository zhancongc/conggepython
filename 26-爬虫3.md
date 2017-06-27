# 爬虫（三）

> 前面两期，我们知道可以借助requests库获取网页信息；并分析了HTML文档的结构，发现了目标数据的定位的思路。本期，我们介绍如何提取数据，获取你想要的下载链接。

我们的目标就是从这个网页：http://m.idyjy.com/sub/13467.html中提取电影的信息。首先，获取网页内容。

```python
import requests
url = "http://m.idyjy.com/sub/13467.html"
r = requests.get(url)
# 指定编码为GBK，简体中文网页的编码一般是GBK，否则会乱码
r.encoding = 'GBK'
# 得到GBK编码的网页文本信息
html = r.text
```

## Beautiful Soup

提取数据需要用到Beautiful Soup，它是一个可以从HTML或XML中提取数据的python库，简称为BS。BS已经发展到了第4版，所以现在用到的是BS4。

按照惯例，需要安装BS4，安装命令是：

```shell
pip install beautifulsoup4
```

BS4能够解析HTML的关键在于解析器，目前有4种主流的解析器：`html.parser`, `lxml`, `xml`, `html5lib`。在文档结构不完整或者有错误的时候，就要考虑使用`html5lib`，它的容错能力最强。本期，我们使用`lxml`，因为它的解析速度最快。

```python
# 引入BeautifulSoup，并给它起一个简称：BS
>>> from bs4 import BeautifulSoup as BS

# 将前面获取到的网页信息html作为参数，得到一个soup对象
# 使用了lxml解析器
>>> soup = BS(html, 'lxml')

```

BS支持`find()`方法和`find_all()`方法搜索网页标签，标签内的属性可以用`get()`方法获取。

```python
# 获取所有的h3标签
>>> soup.find_all('h3')
[<h3>《空即是色/色即是空2015》迅雷下载地址1</h3>, <h3>《空即是色/色即是空2015》迅雷下载地址2</h3>, <h3>《空即是色/色即是空2015》下载说明</h3>, <h3>《空即是色/色即是空2015》详细信息</h3>, <h3>相似推荐</h3>]

# 获取第一个h3标签，如果找不到则不回返回任何信息
>>> soup.find('h3')
<h3>《空即是色/色即是空2015》迅雷下载地址1</h3>

# soup.find('h3')的简写
>>> soup.h3
<h3>《空即是色/色即是空2015》迅雷下载地址1</h3>
```

## 提取数据

分析一下网页，我们发现：class属性是`dramaNumList dramaNumList3 clearfix`的ul标签，它下面的第一个li标签中的a标签，这个a标签`href`属性的值就是我们要的下载链接。

![查找目标链接](http://opa63tcx6.bkt.clouddn.com/pictures/%E6%9F%A5%E6%89%BE%E9%93%BE%E6%8E%A5.png)

对应的代码是：

```python
# 首先根据class名查找目标ul，符合条件的ul有两个
# attrs是attributions的简写，它是一个字典
>>> uls = soup.find_all('ul', attrs={'class': 'dramaNumList dramaNumList3 clearfix'})
# 发现第一个ul才是我们想要的
>>> ul = uls[0]
# 获取下面第一个a标签
>>> a = ul.a
# 使用get()方法获取href属性
>>> a.get('href')
'thunder://QUFlZDJrOi8vfGZpbGV8ob6159OwvNLUsHd3dy5pZHlqeS5jb23PwtTYob/Jq7y0yse/1S4yMDE1LkhEuqvT79bQ19YubXA0fDczMDk2ODIwMXwxOUEzMzRBRjgxQjU3REEzMjc5NjQ5MDIzNjQ1MDA0N3xoPUNaNTI2R0dCNVY0SjdRT1FBWVFaVTNGWTcyUUQ1TDdPfC9aWg=='
```

注意：爬虫的目的就是爬取大量的网页，只能爬取一个网页就失去了爬虫存在的意义。我们要充分研究网页的结构，找到通用的提取信息的思路。这一点，需要大家在实践中慢慢体会。

## UA伪装

信息是一种宝贵的资源，各个网站对爬虫的容忍度都比较低。为了防止辛辛苦苦做出来的爬虫失效，我们需要对爬虫做一定的伪装。下面介绍最简单的一种伪装：UA伪装。

> UA是User Agent的缩写，它是用户客户端的标识，服务器就是通过它来辨别用户的设备的。
>
> 比如：我的手机的UA是：Mozilla/5.0 (iPhone; CPU iPhone OS 10_3_2 like Mac OS X)  AppleWebKit/603.2.4  (KHTML, like Gecko) Version/10.0 Mobile/14F89 Safari/602.1。
>
> 这一串信息从左到右分别是：浏览器标识 (操作系统标识; 加密等级标识; 浏览器语言) 渲染引擎标识 版本信息。

如果我们的requests直接访问网站，它的UA是：python-requests/2.13.0。很显然，这不是一个正常客户端的UA，很容易被反爬虫的网站封杀。即使不被封杀，也不一定能够获取到数据。

> header是HTTP请求的头部。类似于HTML，一个HTTP请求包含了头部和正文。HTTP请求头部包含了请求方法、资源的标识符及使用的协议，正文部分就是HTTP要传输的数据。

UA是header的一项属性，requests提供了修改header的功能。

```python
# 定制一个header
head_connection = ['Keep-Alive','close']
head_accept = ['text/html, application/xhtml+xml, */*']
head_accept_language = ['zh-CN,fr-FR;q=0.5','en-US,en;q=0.8,zh-Hans-CN;q=0.5,zh-Hans;q=0.3']
head_user_agent = [
    '''Mozilla/5.0 (iPhone; CPU iPhone OS 1032 like Mac OS X)  AppleWebKit/603.2.4  
    (KHTML, like Gecko) Version/10.0 Mobile/14F89 Safari/602.1'''
]
header = {
    'Connection': head_connection[0],
    'Accept': head_accept[0],
    'Accept-Language': head_accept_language[1],
    'User-Agent': head_user_agent[0]
}

import requests
url = "http://m.idyjy.com/sub/13467.html"
# 在requests发出HTTP请求时使用我们定制的header
r = requests.get(url, header=header)
```

UA伪装只是初级的伪装，在爬取网站信息的时候，不能太过“嚣张”。是否“嚣张”的重要标志就是：短时间内同一个客户端大量访问。有些服务器会封杀爬虫的ip地址，此时，无论怎么更换UA也是无济于事的。

## 总结

1. Beautiful Soup可以用来解析HTML，获取我们想要的信息
2. 提取信息需要考虑网页的结构，最好找到通用的方法
3. UA伪装可以骗过一些服务器，保证爬虫的安全

扫一扫这个二维码，关注公众号：聪哥python，获取最新python3基础教程 。

![img](http://mmbiz.qpic.cn/mmbiz_jpg/yQOGaRouhVreicib77WTAHp9H3AptSeTuaEiawPVOhthVfTDmOnrKAknibibMH3ADF3KVTUxiabTtDiaiaUrKibYh0QVX0Q/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1)