---
title: 【PythohCrawler】
date: 2022-03-03 18:46:57
tags: 爬虫
---

# Python爬虫

## 爬虫简介
### 1.定义
网络爬虫就是模拟客户端发送网络请求，获取响应数据，一种按照一定的规则，自动的抓取万维网信息的程序或脚本

### 2.爬虫的应用：
随着网络的迅速发展，万维网成为大量信息的载体，如何有效地提取并利用这些信息成为一个巨大的挑战，因此爬虫应运而生，它不仅能够被使用在搜索引擎领域，而且在大数据分析，以及商业领域都得到了大规模的应用。
1. 数据分析
在数据分析领域，网络爬虫通常是搜集海量数据的必备工具。对于数据分析师而言，要进行数据分析，首先要有数据源，而学习爬虫，就可以获取更多的数据源。在采集过程中，数据分析师可以按照自己目的去采集更有价值的数据，而过滤掉那些无效的数据。
2. 商业领域
对于企业而言，及时地获取市场动态、产品信息至关重要。企业可以通过第三方平台购买数据，比如贵阳大数据交易所、数据堂等，当然如果贵公司有一个爬虫工程师的话，就可通过爬虫的方式取得想要的信息。

### 3.编写爬虫的流程：
1. 先由 urllib 模块的 request 方法打开 URL 得到网页 HTML 对象。
2. 使用浏览器打开网页源代码分析网页结构以及元素节点。
3. 通过 Beautiful Soup 或则正则表达式提取数据。
4. 存储数据到本地磁盘或数据库。

------


## request库
Python 内置了 requests 模块，该模块主要用来发送 HTTP 请求并获取响应数据。Requests的主要功能及用途是用作发送网络请求，根据对方服务器的要求不同，可使用GET、POST和PUT等方式进行请求。并且可以对请求头进行伪装、使用代理访问等。

```python
#导入 requests 包
import requests
#发送请求
req = requests.get('https://www.baidu.com/')
print(req.status_code)
# 响应状态码
print(req.text)
# 响应的文本内容
print(req.content)
# 响应的二进制内容
print(response.content.decode())
# 把二进制数据转换字符串
print(req.cookies)
# 响应的cookies
print(req.encoding)
# 响应的编码
print(req.headers)
# 响应的头部信息
print(req.url)
# 响应的网址
print(req.history)
```

#### requests 方法
requests 方法如下表：

|方法|	描述|
|  ----  | ----  |
|delete(url, args)|	发送 DELETE 请求到指定 url|
|get(url, params, args)|	发送 GET 请求到指定 url|
|head(url, args)	|发送 HEAD 请求到指定 url|
|patch(url, data, args)|	发送 PATCH 请求到指定 url|
|post(url, data, json, args)	|发送 POST 请求到指定 url|
|put(url, data, args)	|发送 PUT 请求到指定 url|
|request(method, url, args)	|向指定的 url 发送指定的请求方法|

--------------

## BeautifulSoup库
Beautiful Soup是一个从HTML或XML中提取数据的python库。
它提供一些简单的、python式的函数用来处理导航、搜索、修改分析树等功能。

#### Beautiful Soup库的基本使用

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
```
[详细](https://blog.csdn.net/Eastmount/article/details/109497225?spm=1001.2101.3001.6650.8&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-8-109497225-blog-99338957.pc_relevant_3mothn_strategy_recovery&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-8-109497225-blog-99338957.pc_relevant_3mothn_strategy_recovery&utm_relevant_index=12)
1. 对象介绍与创建：
对象：
代表要解析整个文档树，它支持遍历文档树和搜索文档树中描述的大部分的方法。
创建beautifulsoup对象：
```python
from bs4 import beautifulsoup
soup=beautifulsoup=(html,'lxml') #后面的lxml是指定的解析器
```

2. 获取网页标签信息
```python
soup.title
soup.head
soup.a
```

3. 搜索文档树
对象的find方法：
```python
find(self,name=Mone,attrs={},recursive=True,text=None,**kwargs) 
参数:
    name:标签名
    attrs:属性字典
    recursibe:是否递归循环查找
    text:根据文本内容查找
    可以指定上述参数的值来查找内容
```
------------


## 正则表达式
正则表达式是对字符串操作的一种逻辑公式，就是用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“规则字符串”，这个“规则字符串”用来表达对字符串的一种过滤逻辑。
正则表达式的大致匹配过程是：
1. 依次拿出表达式和文本中的字符比较，
2. 如果每一个字符都能匹配，则匹配成功；一旦有匹配不成功的字符则匹配失败。
3. 如果表达式中有量词或边界，这个过程会稍微有一些不同。

#### 正则表达式的语法规则

![](1.png)
```python
#导入正则模块
import re
#字符匹配
rs=re.findall('abc','awgduywagydgaiabc')
rs=re.findall('a.c',)
语法：
1.匹配除换行符以外的所有字符
2.\d匹配[0-9]的数字
3.\w匹配字母数字——和中文
4.*前面的一个匹配模式出现0次或多次
5.+前面的一个匹配模式出现1次或多次
6.？前面的一个匹配模式出现0或1次
```
```python
re.findall(pattern,string,flags=0)
作用：扫描整个string字符串，返回所有与pattern匹配的列表
参数：
pattern:正则表达式
string:从那个字符串中查找
flags:指定 匹配模式
返回string中与pattern匹配的结果列表
```
特点：
如果正则表达式中没有（）则返回整个正则匹配的列表
如果正则表达式中有（），则返回（）中匹配的内容列表，小括号两边东西都是负责确定提取数据的所在位置
#### 反斜杠问题
与大多数编程语言相同，正则表达式里使用”\”作为转义字符，这就可能造成反斜杠困扰。假如你需要匹配文本中的字符”\”，那么使用编程语言表示的正则表达式里将需要4个反斜杠”\\\\”：前两个和后两个分别用于在编程语言里转义成反斜杠，转换成两个反斜杠后再在正则表达式里转义成一个反斜杠。


--------


## json模块
用于json和python数据之间的相互转换
|函数|描述|
|---|---|
|json.dumps	|将 Python 对象编码成 JSON 字符串|
|json.loads	|将已编码的 JSON 字符串解码为 Python 对象|

#### 将 Python 对象编码成 JSON 字符串
```python
import json
data = [ { 'a' : 1, 'b' : 2, 'c' : 3, 'd' : 4, 'e' : 5 } ]
data2 = json.dumps(data)
print(data2) 

以上代码执行结果为：
[{"a": 1, "c": 3, "b": 2, "e": 5, "d": 4}]
```
#### 把json格式文件转换为Python类型数据
```python
import json

jsonData = '{"a":1,"b":2,"c":3,"d":4,"e":5}';

text = json.loads(jsonData)
print(text)
以上代码执行结果为：
{u'a': 1, u'c': 3, u'b': 2, u'e': 5, u'd': 4}
```



--------


# 爬取丁香园疫情数据
```python
#1.导入模块
import requests
from bs4 import beautifulsoup
import re
#2.发送请求，获取响应
response = requests.get('https://ncov.dxy.cn/ncovh5/view/pneumonia')
#3.从响应中获取数据
homepage = response.content.decode()
#4.使用beautifulsoup提取疫情数据
soup=beautifulsopu(homepage,'lxml')
script=soup.find(id='')
text=script.text

#5.使用正则表达式，提取Json字符串
匹配括号中的内容
json_str=re.findall(r'\[.+\]d',text)[0]

#6.json格式字符串转换为Python类型
last_day_cor=json.loads(json_str) #python列表，里面是字典

#7.以json格式保存
with open(''.'w') as fp:
    json.dump(last_day,fp)

```
