---
title: 问题汇总
date: 2016-09-01 18:22:06
tags:
---
1、描述一下你只做一个网页的工作流程。
1）根据需求，确定主题。透彻深入所做网站的核心功能和关键。
2）收集资料。从对比相同类型的网站（惯用而熟悉的样式，用户更乐意接受），参照别人可行的实现方法。
3）规划网站。抽离出类似的模块和可重用的部件。如果是响应式网站就需要设定断点，根据不同宽度屏幕设定样式。
4）设计数据库。
5）搭建基本的框架。引入重置样式表reset.css和字体样式表font.css，风格统一的图标还有后台需要用到的包。
6）编码和调试。注意统一命名和编码规范。当多人开发时，还需要制定规范文档。
7）上传测试。利用FTP工具，把网站发布到自己申请的主页存放服务器上。网站上传以后，你要在浏览器中打开自己的网站，逐页逐个链接的进行测试，发现问题，及时修改，然后再上传测试。
8）推广宣传 。不断宣传，提高网站的访问率和知名度。推广的方法有很多，例如到搜索引擎上注册、与别的网站交换链接、加入广告链等。
9）维护更新 。网站要注意经常维护更新内容，保持内容的新鲜，不要一做好就放在那儿不变了，只有不断地给它补充新的内容，才能够吸引住浏览者



2、你如何对网站的文件和资源进行优化？

https://segmentfault.com/a/1190000002956639

一共18条，很详细。几乎涵盖所有网站资源优化的内容。





3、如何规避JavaScript多人开发函数重名问题？

    闭包，沙箱模式
    js模块化mvc（数据层、表现层、控制层）
    seajs（如果了解的呢，可以说）
    变量转换成对象的属性
    对象化



4、请尽可能详尽的解释AJAX的工作原理。

Ajax的工作原理相当于在用户和服务器之间加了—个中间层(AJAX引擎),使用户操作与服务器响应异步化。并不是所有的用户请求都提交给服务器,像—些数据验证和数据处理等都交给Ajax引擎自己来做, 只有确定需要从服务器读取新数据时再由Ajax引擎代为向服务器提交请求。

Ajax其核心有JavaScript、XMLHTTPRequest、DOM对象组成，通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用JavaScript来操作DOM而更新页面。这其中最关键的一步就是从服务器获得请求数据。让我们来了解这几个对象。

(1).XMLHTTPRequest对象

Ajax的一个最大的特点是无需刷新页面便可向服务器传输或读写数据(又称无刷新更新页面),这一特点主要得益于XMLHTTP组件XMLHTTPRequest对象。

(2).JavaScript

JavaScript是一在浏览器中大量使用的编程语言。

(3).DOM Document Object Model

DOM是给HTML和XML文件使用的一组API。它提供了文件的结构表述，让你可以改变其中的內容及可见物。其本质是建立网页与Script或程序语言沟通的桥梁。所有WEB开发人员可操作及建立文件的属性、方法及事件都以对象来展现（例如，document就代表“文件本身“这个对像，table对象则代表HTML的表格对象等等）。这些对象可以由当今大多数的浏览器以Script来取用。一个用HTML或XHTML构建的网页也可以看作是一组结构化的数据，这些数据被封在DOM（Document Object Model）中，DOM提供了网页中各个对象的读写的支持。

(4).XML

可扩展的标记语言（Extensible Markup Language）具有一种开放的、可扩展的、可自描述的语言结构，它已经成为网上数据和文档传输的标准,用于其他应用程序交换数据 。

(5).综合

Ajax引擎，实际上是一个比较复杂的JavaScript应用程序，用来处理用户请求，读写服务器和更改DOM内容。JavaScript的Ajax引擎读取信息，并且互动地重写DOM，这使网页能无缝化重构，也就是在页面已经下载完毕后改变页面内容，这是我们一直在通过JavaScript和DOM在广泛使用的方法，但要使网页真正动态起来，不仅要内部的互动，还需要从外部获取数据，在以前，我们是让用户来输入数据并通过DOM来改变网页内容的，但现在，XMLHTTPRequest，可以让我们在不重载页面的情况下读写服务器上的数据，使用户的输入达到最少。

Ajax使WEB中的界面与应用分离（也可以说是数据与呈现分离），而在以前两者是没有清晰的界限的，数据与呈现分离的分离，有利于分工合作、减少非技术人员对页面的修改造成的WEB应用程序错误、提高效率、也更加适用于现在的发布系统。也可以把以前的一些服务器负担的工作转嫁到客户端，利于客户端闲置的处理能力来处理。



5、你使用过哪些JavaScript库？

jQuery

animate

Bootstrap

zepto

artTemplate

normalize：它在默认的HTML元素样式上提供了跨浏览器的高度一致性。

Swiper

fullpage：jQuery全屏滚动插件。

了解更多：

https://www.evget.com/article/2013/9/22/19657.html
