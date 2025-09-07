---
title: 前端面试-HTML篇
date: 2025-08-23 18:09:00
tags: 前端面试
---
## 1. 什么是DOM和BOM？

1. DOM (Document Object Model):
   * DOM是表示HTML和XML文档的标准的对象模型。它将文档中的每个组件都看作一个对象，可以用js操控DOM去动态改变 元素 结构 样式。
   * DOM 以树状结构组织文档的内容，其中树的根节点是 `document` 对象，代表整个文档。对象有各种方法和属性，可以用来访问和修改文档的内容和结构。
2. BOM (Browser Object Model):
   * BOM 是浏览器提供的对象模型，用于操作浏览器窗口。BOM 提供了与浏览器窗口交互的接口，如窗口大小、滚动位置、历史记录、cookie 等。

  DOM 是用于访问和操作网页文档的对象模型，而 BOM 是用于控制浏览器窗口及其各个组件的对象模型。
  在 JavaScript 编程中，开发者通常会同时使用 DOM 和 BOM 来完成各种任务，如操作网页元素、导航控制、事件处理等。

---

## 2. 网址从输入到页面显示的全过程？

* DNS解析域名
* 发起TCP链接
* 发送HTTP请求
* 服务器处理请求并返回HTTP报文
* 浏览器解析并渲染页面
* 链接结束

---

## 3. 浏览器解析渲染页面流程？

![浏览器解析渲染页面流程](3-1.png)

* 解析HTML形成DOM树
* 解析CSS形成CSSOM树
* 加载JS，执行JS代码
* 合并DOM树和CSSOM树，形成Render Tree渲染树
* 计算元素位置进行页面布局
* 浏览器开始渲染并绘制页面

---

## 4. 行内元素有哪些？块级元素有哪些？空元素有哪些

![ ](4-1.png)

---

## 5. 怎么实现点击回到顶部功能？

1. 锚点
   在页面顶部放置一个指定名称的元素，比如 `<a name="top"></a>`，之后在页面任意位置放置一个链接，比如 `<a href="#top">回到顶部</a>`，点击链接即可回到顶部。
2. scrollTop()
   scrollTop() 可以获取或设置元素内容从其顶部边缘滚动的像素数。
   通过 `document.body.scrollTop=0` 或 `document.documentElement.scrollTop=0` 来实现回到顶部。
3. scrollTo()
   scrollTo(0,0)方法可以使界面滚动到给定元素的指定坐标位置。

   * 方法一: element.scrollTo(x-coord, y-coord)
     *x-coord 是期望滚动到位置水平轴上距元素左上角的像素。
     y-coord 是期望滚动到位置竖直轴上距元素左上角的像素。*
   * 方法二: element.scrollTo(options)
     *options是一个对象:
     left (number类型)
     top(number类型)
     behavior: ‘smooth’ (平滑过渡效果)*
4. scrollBy()
   scrollBy(xnum,ynum) 则是从当前位置滚动到某个相对位置，从当前位置起向右和向下滚动多少像素。
   将当前位置的滚动长度作为参数，逆向滚动，传递给scrollBy()方法，即可实现回到顶部。

   ```javascript
   var top = document.body.scrollTop;
   scrollBy(0,-top);
   ```
5. scrollIntoView()

   ![ ](5-1.png)

---

## 6. SEO相关？

1. SEO是什么？
   SEO（Search Engine Optimization）搜索引擎优化，是为了从搜索引擎中获取流量，提高网站排名。
2. SEO原理
   * 爬行和抓取
     爬虫抓取网页
   * 索引
     将抓取的网页解析存入数据库
   * 搜索词处理
     对用户的搜索关键词分词处理
   * 排序
     搜索引擎排序程序开始工作，对包含搜索词的网页按照评分进行排序
3. SEO优化
   ![ ](6-1.png)

---

## 7. label标签作用？

label标签用于为表单元素提供标注，比如input标签，select标签，textarea标签。可以将label标签与input标签关联起来，这样点击label标签，就会触发input标签的点击事件。
给 `<input>`一个 id 属性。而 `<label>` 需要一个 for 属性，其值和 `<input>` 的 id 一样。

```html
<label for="cheese">Do you like cheese?</label>
<input type="checkbox" name="cheese" id="cheese" />
```

---

## 8. CSSOM树和DOM树是同时解析的吗？

CSSOM树和DOM树是同时解析的。浏览器会先解析HTML，将HTML解析成DOM树，然后解析CSS，将CSS解析成CSSOM树，解析CSS的过程不会阻塞，但如果遇到了JS脚本的时候CSSOM还没有构建完毕，则需等待CSSOM构建完毕后再去执行JS脚本，然后再执行DOM解析。最后将DOM树和CSSOM树合并，生成Render Tree。

---

## 9. HTML的生命周期？

1. DOMContentLoaded —— 浏览器已完全加载 HTML，并构建了 DOM 树，当 DOM 准备就绪时，document 上的 DOMContentLoaded 事件就会被触发。在这个阶段，我们可以将 JavaScript 应用于元素。
   诸如 `<script>`...`</script>` 或 `<script src="..."></script>` 之类的脚本会阻塞 DOMContentLoaded，浏览器将等待它们执行结束。
   图片和其他资源仍然可以继续被加载。
   浏览器会在这里进行自动填充表单。
2. load —— 浏览器不仅加载完成了 HTML，还加载完成了所有外部资源：图片，样式等。
3. beforeunload/unload —— 当用户正在离开页面时。

---

## 10. meta标签中的viewport属性有什么用？

viewport 是一个元标签，用于定义网页的视口，即网页可见区域大小。
手机浏览器是把页面放在一个viewport中，然后缩放，达到一个最佳的显示效果。用户可以通过平移和缩放来浏览页面。
一个常用的针对移动网页优化的页面宽度设置是：`<meta name="viewport" content="width=device-width">`。
![ ](10-1.png)

---

## 11.style写在body前后有什么区别？

页面加载自上而下，style标签写在body之前，会优先加载，并应用到页面。而style标签写在body之后，当解析到写在尾部的样式表会导致浏览器停止之前的渲染，等到加载解析样式表后重新渲染，可能会出现页面闪烁问题。

---

## 12.如何进行网站的性能优化？

1. content 方面
   * 减少 HTTP 请求：合并文件、 CSS 精灵、 inline Image
   * 减少 DNS 查询： DNS 缓存、将资源分布到恰当数量的主机名
   * 减少 DOM 元素数量
2. Server 方面
   * 使用 CDN
   * 配置 ETag
   * 对组件使用Gzip 压缩
3. Cookie 方面
   * 减小 cookie 大小
4. css 方面
   * 将样式表放到页面顶部
   * 不使用 CSS 表达式
   * 使用 `<link>` 不使用@import
5. Javascript 方面
   * 将脚本放到页面底部
   * 将 javascript 和 css 从外部引⼊
   * 压缩 javascript 和 css
   * 删除不需要的脚本

---

## 13.HTTP状态码及其含义

1. 1XX ：信息状态码
   100 Continue 继续，⼀般在发送 post 请求时，已发送了 http header 之后服务端将返回此信息，表示确认，之后发送具体参数信息
2. 2XX ：成功状态码
   200 OK 正常返回信息
   201 Created 请求成功并且服务器创建了新的资源
   202 Accepted 服务器已接受请求，但尚未处理
3. 3XX ：重定向
   301 Moved Permanently 请求的网页已永久移动到新位置。
   302 Found 临时性重定向。
   303 See Other 临时性重定向，且总是使⽤ GET 请求新的 URI 。
   304 Not Modified ⾃从上次请求后，请求的⽹⻚未修改过。
4. 4XX ：客户端错误
   400 Bad Request 服务器⽆法理解请求的格式，客户端不应当尝试再次使⽤相同的内容发起请求。
   401 Unauthorized 请求未授权。
   403 Forbidden 禁⽌访问。
   404 Not Found 找不到如何与 URI 相匹配的资源。
5. 5XX: 服务器错误
   500 Internal Server Error 最常见的服务器端错误。
   503 Service Unavailable 服务器端暂时⽆法处理请求（可能是过载或维护

---

## 14.请描述⼀下 cookies，sessionStorage和localStorage 的区别？

1. cookies:
   cookies，它是网站为了标识用户身份而储存在用户本地终端上的数据。
   cookies数据始终在同源的http请求中携带。会在浏览器和服务器间来回传递。
   有设置的到期时间，到期之前一直有效。
2. sessionStorage:
   用于临时保存同一窗口（或标签页）的数据，在关闭窗口或标签页之后将会删除这些数据。
3. localStorage:
   用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。
4. 存储大小：
   cookies: 4K
   sessionStorage/localStorage: 5M

---

## 15.在css/js代码上线之后开发人员经常会优化性能，从用户刷新网页开始，一次js请求⼀般情况下有哪些地方会有缓存处理？

dns 缓存， cdn 缓存，浏览器缓存，服务器缓存

---

## 16.行内元素和块内元素有什么区别？

![ ](16-1.png)

---
