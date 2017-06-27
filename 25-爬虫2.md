# 电影爬虫（二）

>上期我们知道借助requests库，可以得到目标网页的内容。但是，网页内容那么多，我们要的仅仅是下载链接、种子或者磁力链接。这就涉及到提取目标数据的问题，本期我们就来探讨一下有效数据在网页的什么地方。告诉你一个小秘密，本期有福利哦！

## HTML

想要从网页中提取目标数据，就必须要了解网页本身。网页是由HTML、CSS和JavaScript组成的，HTML负责展示网页内容，CSS控制页面元素的样式，JavaScript控制网页元素交互。不难看出，我们需要的数据都在HTML中。

HTML的结构十分简单，主要分为head部分和body部分。head部分主要是网页的一些属性和引入的一些文件，这些信息中没有我们想要的电影种子。

即使定位到了body部分依然不够，因为body部分依然很多内容，这些信息都是用HTML标签来标记的。body部分有很多标签，但是需要重点掌握的只有以下几种：

| 标签                               | 说明         |
| -------------------------------- | ---------- |
| `<h1></h1><h2></h2>...<h5></h5>` | 从一级标题到五级标题 |
| `<div></div>`                    | 块级元素       |
| `<span></span>`                  | 内联元素       |
| `<p></p>`                        | 段落         |
| `<a></a>`                        | 超链接        |
| `<img></img>`                    | 图片         |
| `<ul></ul>`                      | 无序列表       |
| `<ol><ol>`                       | 有序列表       |
| `<li></li>`                      | 列表中的条目     |
| `<table></table>`                | 表格         |
| `<tr></tr>`                      | 表格的行       |
| `<td></td>`                      | 表格的列       |

没有学过HTML的读者看着会有点懵，其实这些东西都记不住也没关系。毕竟我们讲的是爬虫，我们只要知道：目标数据在什么标签下面就可以了，并不需要知道这个标签有什么含义。

## HTML标签的特征

下面是电影家园里面的某部电影的网页截图，和平时我们见到的网页不同的是，这个页面打开了调试模式[[1]](参考资料)。

![查找目标链接](http://opa63tcx6.bkt.clouddn.com/pictures/%E6%9F%A5%E6%89%BE%E9%93%BE%E6%8E%A5.png)

通过调试模式，我们很容易发现第一个迅雷下载的链接，然后通过迅雷我们就能搞到这个电影啦。

但是，作为资深看片老司机，一部电影怎么能满足我们呢？如果要下载多部电影，每部都要这样手动复制链接去下载，肯定是不理想的方法。既然是讲电影爬虫，我们自然有更为强大的技术手段去搞定它。突破点就在于HTML上，为了准确找到下载链接所在的HTML标签，我们需要掌握HTML标签的一些特征。

下面我们观察一段HTML代码，尝试找到一些特征。

```html
<!-- 注意这里的class 和 id ，它是html标签的重要特征 -->
<ul class="tabNav clearfix" id="detail_tab_nav">
    <li>
        <a class="cur">
            <!-- 注意这里的 h3 标签，它是这段代码中唯一的 h3 标签 -->
            <h3>《空即是色/色即是空2015》迅雷下载地址1</h3>
        </a>
    </li>
</ul>
<ul class="dramaNumList dramaNumList3 clearfix">
  <li><a href=
"thunder://QUFlZDJrOi8vfGZpbGV8ob6159OwvNLUsHd3dy5pZHlqeS5jb23PwtTYob/Jq7y0yse/1S4yMDE1LkhEuqvT79bQ19YubXA0fDczMDk2ODIwMXwxOUEzMzRBRjgxQjU3REEzMjc5NjQ5MDIzNjQ1MDA0N3xoPUNaNTI2R0dCNVY0SjdRT1FBWVFaVTNGWTcyUUQ1TDdPfC9aWg==" target="_self">色即是空.2015.HD韩语中字.mp4 (696MB)</a></li>
  <li><a href=
"thunder://QUFlZDJrOi8vfGZpbGV8ob6159OwvNLUsHd3dy5pZHlqeS5jb23PwtTYob/Jq7y0yse/1TIwMTUuSES6q9Pv1tDX1i5tcDR8MTA3OTk2OTc2N3w1MDg0NURDOTk3Q0NDOENGQUQ3NUYwNkYxQkQ3OTUwRHxoPVgyNlBGTFdNVldGQzREQ0hBQ083UlVQR1RQN1BBRlVGfC9aWg==" target="_self">色即是空2015.HD韩语中字.mp4 (1.01GB)</a></li>
</ul>
```

不难发现，有的标签有class，有的标签有id，有的标签内部有文本内容，有的标签本身就比较特殊。这些都是HTML标签的重要特征，它们可以帮助我们找到目标内容。

除此之外，各个标签之间的相对位置也是一个重要的线索。比如：`class`属性是`dramaNumList dramaNumList3 clearfix`的`ul`标签，找到它下面的第一个`li`标签中的`a`标签，这个`a`标签`href`属性的值就是我们目标数据——电影的下载链接。

上面介绍的定位方法就是XPath方法。由于篇幅限制，就不再详细介绍，感兴趣的读者可以参考菜鸟教程的XPath教程[[1]](http://www.runoob.com/xpath/xpath-tutorial.html)，链接：http://www.runoob.com/xpath/xpath-tutorial.html。

## 总结

1. 网页由head部分和body部分组成，我们需要关注的是body部分
2. body部分由很多标签组成的，目标信息在某个标签下面
3. 为了定位标签，我们需要注意标签的一些属性特征和位置特征

## 参考资料

1. 这个截图来自chrome，它是基于开源项目chromium发展而来的。凡是chromium内核的浏览器，比如：百度浏览器、360浏览器、QQ浏览器等，只需要使用快捷键F12。此外，也可以使用组合键Ctrl+Shift+I，或者右击页面，选择检查。
2. 《XPath》http://www.runoob.com/xpath/xpath-tutorial.html





