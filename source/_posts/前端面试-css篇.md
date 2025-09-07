---
title: 前端面试-CSS篇
date: 2025-08-26 20:56:12
tags:
---
## 1.让一个元素水平垂直居中的方案？

![ ](1-1.png)

---

## 2.说说对盒子模型的理解？

![ ](2-1.png)

一个盒子由四个部分组成：content、padding、border、margin

1. 标准盒模型：

   * 盒子总宽度 = width + padding + border + margin;
   * 盒子总高度 = height + padding + border + margin
2. IE怪异盒模型：

   * 盒子总宽度 = width + margin;
   * 盒子总高度 = height + margin;

---

## 3.如何创建块级格式化上下文(block formatting context),BFC有什么用？

1. 创建规则：

   * 根元素
   * 浮动元素（ float 不取值为 none ）
   * 绝对定位元素（ position 取值为 absolute 或 fixed ）
   * display 取值为 inline-block 、 table-cell 、 table-caption 、 flex 、inline-flex 之⼀的元素
   * overflow 不取值为 visible 的元素
2. 作⽤：

   * 可以包含浮动元素
   * 不被浮动元素覆盖
   * 阻⽌父子元素的 margin 折叠

---

## 4.display有哪些值？说明他们的作用

* block 转换成块状元素。
* inline 转换成⾏内元素。
* none 设置元素不可⻅。
* inline-block 像⾏内元素⼀样显示，但其内容像块类型元素⼀样显示。
* list-item 像块类型元素⼀样显示，并添加样式列表标记
* table 此元素会作为块级表格来显示
* inherit 规定应该从⽗元素继承 display 属性的值

---

## 5. position的值， relative和absolute定位原点是？

* absolute ：⽣成绝对定位的元素，相对于 static 定位以外的第⼀个⽗元素进⾏定位
* fixed ：⽣成绝对定位的元素，相对于浏览器窗⼝进⾏定位
* relative ：⽣成相对定位的元素，相对于其正常位置进⾏定位
* static 默认值。没有定位，元素出现在正常的流中
* inherit 规定从⽗元素继承 position 属性的值

---

## 6. ::before 和 :hover中双冒号和单冒号 有什么区别？解释一下这2个

伪元素的作用
单冒号( : )用于 CSS3 伪类，双冒号( :: )用于 CSS3 伪元素
⽤于区分伪类和伪元素。

---

## 7.CSS不同选择器的权重(CSS层叠的规则)

* ! important 规则最重要，大于其它规则
* 行内样式规则，加 1000
* 对于选择器中给定的各个 ID 属性值，加 100
* 对于选择器中给定的各个类属性、属性选择器或者伪类选择器，加 10
* 对于选择其中给定的各个元素标签选择器，加1
* 如果权值⼀样，则按照样式规则的先后顺序来应⽤，顺序靠后的覆盖靠前的规则

---

## 8.CSS在性能优化方面的实践？

* css 压缩与合并、 Gzip 压缩
* css 文件放在 head 里、不要用 @import
* 尽量用缩写、避免用滤镜、合理使用选择器

---

## 9.base64 的使用？

* ⽤于减少 HTTP 请求
* 适⽤于⼩图⽚
* base64 的体积约为原图的 4/3

---

## 10. 水平居中的方法？

* 元素为⾏内元素，设置⽗元素 text-align:center
* 如果元素宽度固定，可以设置左右 margin 为 auto ;
* 如果元素为绝对定位，设置⽗元素 position 为 relative ，元素设left:0;right:0;margin:auto;
* 使用 flex-box 布局，指定 justify-content 属性为center
display 设置为 tabel-ceil

---

## 11. 垂直居中的方法？

* 将显示方式设置为表格， display:table-cell ,同时设置 vertial-align：middle
* 使用 flex 布局，设置为 align-item：center
* 绝对定位中设置 bottom:0,top:0 ,并设置 margin:auto
* 绝对定位中固定高度时设置 top:50%，margin-top 值为⾼度⼀半的负值
* 文本垂直居中设置 line-height 为 height 值

---

## 12. 怎样做移动端的适配？

1. 响应式设计

   * 使用css媒体查询来根据设备特征应用不同样式
   * 通过设置百分比宽度、最大宽度或相对单位来确保元素相对于其容器的大小进行自适应。

  ![ ](12-1.png)
2. 弹性布局和网格布局
3. 移动端优先，首先定义移动端的样式，然后使用媒体查询逐步添加到更大屏幕上，以确保基本功能在小屏幕上正常工作。

---
