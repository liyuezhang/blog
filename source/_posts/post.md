---
title: JavaScript的一些知识点
date: 2016-10-01 20:16:17
tags:
---
# 1.执行上下文：

单线程

同步执行，只有栈顶的上下文处于执行中，其他上下文需要等待

全局上下文只有唯一的一个，它在浏览器关闭时出栈

函数的执行上下文的个数没有限制

每次某个函数被调用，就会有个新的执行上下文为其创建，即使是调用的自身函数，也是如此

- 创建阶段
  在这个阶段中，执行上下文会分别创建变量对象，建立作用域链，以及确定this的指向
- 代码执行阶段
  创建完成之后，就会开始执行代码，这个时候，会完成变量赋值，函数引用，以及执行其他代码。
  变量对象的创建，依次经历了以下几个过程。
  1. 建立arguments对象。检查当前上下文中的参数，建立该对象下的属性与属性值。
  2. 检查当前上下文的函数声明，也就是使用function关键字声明的函数。在变量对象中以函数名建立一个属性，属性值为指向该函数所在内存地址的引用。如果函数名的属性已经存在，那么该属性将会被新的引用所覆盖。
  3. 检查当前上下文中的变量声明，每找到一个变量声明，就在变量对象中以变量名建立一个属性，属性值为undefined。如果该变量名的属性已经存在，为了防止同名的函数被修改为undefined，则会直接跳过，原属性值不会被修改.

# 2.变量对象和活动对象有什么区别，他们其实都是同一个对象，只是处于执行上下文的不同生命周期

    demo1:
    function test() {
        console.log(a);
        console.log(foo());

        var a = 1;
        function foo() {
            return 2;
        }
    }

    test();

function声明会比var声明优先级更高一点，所以在执行过程中顺序就变成了：

    function test() {
        function foo() {
            return 2;
        }
        var a;
        console.log(a);
        console.log(foo());
        a = 1;
    }

    test();

    demo2:
    function test() {
        console.log(foo);
        console.log(bar);

        var foo = 'Hello';
        console.log(foo);
        var bar = function () {
            return 'world';
        }

        function foo() {
            return 'hello';
        }
    }

    test();//console.log(foo)会打印函数体，console.log(foo())才打印函数执行结果。


# 3.JavaScript代码的整个执行过程，分为两个阶段，代码编译阶段与代码执行阶段。编译阶段由编译器完成，将代码翻译成可执行代码，这个阶段作用域规则会确定。执行阶段由引擎完成，主要任务是执行可执行代码，垃圾回收，执行上下文在这个阶段创建

# 4.作用域链是由一系列变量对象组成，我们可以在这个单向通道中，查询变量对象中的标识符(变量名或函数名)，这样就可以访问到上一层作用域中的变量了。
---



# 3.闭包：

闭包与作用域链息息相关；

闭包是在函数执行过程中被确认。

闭包的应用场景

接下来，我们来总结下，闭包的常用场景。

延迟函数setTimeout

我们知道setTimeout的第一个参数是一个函数，第二个参数则是延迟的时间。在下面例子中，

      function fn() {
      console.log('this is test.')
      }
      var timer =  setTimeout(fn, 1000);
      console.log(timer);



按道理来说，既然fn被作为参数传入了setTimeout中，那么fn将会被保存在setTimeout变量对象中，setTimeout执行完毕之后，它的变量对象也就不存在了。可是事实上并不是这样。至少在这一秒钟的事件里，它仍然是存在的。这正是因为闭包。

---



# 4.this的指向

this的指向，是在函数被调用的时候确定的。也就是执行上下文被创建时确定的。

    var a = 10;
    var obj = {
        a: 20
    }

    function fn () {
        console.log(this.a);
    }

    fn(); // 10
    fn.call(obj); // 20

    var a = 20;
    var foo = {
        a: 10,
        getA: function () {
            return this.a;
        }
    }
    console.log(foo.getA()); // 10

    var test = foo.getA;
    console.log(test());  // 20

foo.getA()中，getA是调用者，他不是独立调用，被对象foo所拥有，因此它的this指向了foo。而test()作为调用者，尽管他与foo.getA的引用相同，但是它是独立调用的，因此this指向undefined，在非严格模式，自动转向全局window。


      // demo03
     var a = 20; 
     var obj = {
     a: 10,
     c: this.a + 20,

    fn: function () {

        return this.a;

    }
    } 
    console.log(obj.c);

在一个函数上下文中，this由调用者提供，由调用函数的方式来决定。如果调用者函数，被某一个对象所拥有，那么该函数在调用时，内部的this指向该对象。如果函数独立调用，那么该函数内部的this，则指向undefined。但是在非严格模式中，当this指向undefined时，它会被自动指向全局对象。

当obj在全局声明时，无论obj.c在什么地方调用，这里的this都指向全局对象，而当obj在函数环境中声明时，这个this指向undefined，在非严格模式下，会自动转向全局对象。

# 5.使用call，apply显示指定this

JavaScript内部提供了一种机制，让我们可以自行手动设置this的指向。它们就是call与apply。所有的函数都具有着两个方法。它们除了参数略有不同，其功能完全一样。它们的第一个参数都为this将要指向的对象。

如下例子所示。fn并非属于对象obj的方法，但是通过call，我们将fn内部的this绑定为obj，因此就可以使用this.a访问obj的a属性了。这就是call/apply的用法。

    function fn() {
        console.log(this.a);
    }
    var obj = {
        a: 20
    }

    fn.call(obj);

而call与applay后面的参数，都是向将要执行的函数传递参数。其中call以一个一个的形式传递，apply以数组的形式传递。这是他们唯一的不同。

    function fn(num1, num2) {
        console.log(this.a + num1 + num2);
    }
    var obj = {
        a: 20
    }

    fn.call(obj, 100, 10); // 130
    fn.apply(obj, [20, 10]); // 50

# 6.http协议：
超文本传输协议(HTTP，HyperText Transfer Protocol)

是互联网上应用最为广泛的一种网络协议。所有的WWW文件都必须遵守这个标准。设计HTTP最初的目的是为了提供一种发布和接收HTML页面的方法。

通常，由HTTP客户端发起一个请求，建立一个到服务器指定端口（默认是80端口）的TCP连接。HTTP服务器则在那个端口监听客户端发送过来的请求。一旦收到请求，服务器（向客户端）发回一个状态行，比如"HTTP/1.1 200 OK"，和（响应的）消息，消息的消息体可能是请求的文件、错误消息、或者其它一些信息。

HTTP使用TCP而不是UDP的原因在于（打开一个）一个网页必须传送很多数据，而TCP协议提供传输控制，按顺序组织数据，和错误纠正。具体细节请参考‘TCP和UDP的不同’。

通过HTTP或者HTTPS协议请求的资源由统一资源定位器（Uniform Resource Identifiers）（或者，更准确一些，URLs）来标识。


请求行和标题必须以<CR><LF> 作为结尾（也就是，回车然后换行）。空行内必须只有<CR><LF>而无其他空格。在HTTP/1.1 协议中，所有的请求头，除Host外，都是可选的。

我们知道，Internet的基本协议是TCP/IP协议，然而在TCP/IP模型最上层的是应用层（Application layer），它包含所有高层的协议。高层协议有：文件传输协议FTP、电子邮件传输协议SMTP、域名系统服务DNS、网络新闻传输协议NNTP和HTTP协议等。

　　HTTP协议（HyperText Transfer Protocol，超文本传输协议）是用于从WWW服务器传输超文本到本地浏览器的传送协议。它可以使浏览器更加高效，使网络传输减少。它不仅保证计算机正确快速地传输超文本文档，还确定传输文档中的哪一部分，以及哪部分内容首先显示(如文本先于图形)等。这就是你为什么在浏览器中看到的网页地址都是以http://开头的原因。



# 7.ajax:

1.同步：客户端浏览器先发起请求，等待响应，服务器返回响应，客户端才会渲染内容，也就是说客户端每一次发起请求的时候都会把之前的内容删掉，等待服务器给下一次响应

2.异步：客户端有部分可见，发起请求，服务器返回响应，客户端把更新后的内容追加到dom树上，此时客户端和服务器是同时运行的，浏览器在浏览网页的同时服务器会返回新的内容

	
原生ajax:
	

	var xhr = new XMLHttpRequest();
	xhr.onreadystatechange = function(){
	if(xhr.readyState==4&&xhr.status==200){  //响应完成且成功
	xhr.open（“请求方法”，url，true)
	xhr.send（null/请求主体）//请求主体（键值对，文件上传，json格式）,get没有请求主体，post要设置头部信息
	）
	}








jquery的ajax:
    $.ajax({

  type:'请求方法'，

  url:'请求地址'，

  data:'请求数据'（可以是对象，或者是键值对的形式），

  success:fn  (成功的回调函数)，

  error:fn,

  beforeSend:fn,  (请求发送之前的回调)，

  complete:fn   (不管成功还是失败都要执行的回调)

})

# 8.跨域问题：

指从一个url上的资源访问另一个url上的资源，这两个资源不在同一个域上，如协议名，域名，端口号不同，这就叫跨域

浏览器哪些情况下允许跨域：

带src和href属性的标签，如srcript,img,link,iframe

所有浏览器都默认禁止xhr对象的跨域请求----处于“同源安全策略”，不允许放在当前的dom树上。

面试题：

我们公司项目项目很大，为了功能上的清晰划分，在static.tedu.cn上放置了所有的静态资源，在data.tedu.cn上放置了所有的动态数据（json格式），请问如何在static.tedu.cn上获取data.tedu.cn/newlist.json上的动态数据？如何绕过浏览器的xhr跨域限制？

答案：1.CORS方案： （跨源的资源共享）cross origin  Resource  Share（适用于自己可以控制动态页面的编写）

在被访问的资源响应中添加如下的头部：

设置一个响应头部（访问控制允许来源）header('Access-Control-Allow-Origin:http://127.0.0.1');

允许从指定的域名发起跨域请求。

2.jsonp方案：JSON with padding,填充式json，是一种使用json数据的手段、方式。

思路：使用动态创建的script标签代替xhr发起异步的请求，获取跨域的数据，在本地的Js中加以处理.

<script async="true" src="跨域地址/x.php"> </script>  //async=true代表异步为真，异步请求

---

    <script>
    function doResponse(data){
    处理服务器端返回的data中的数据...
    }

    </script>

服务器端返回的数据形如：

Content-Type:application/javascript

doResponse({x:y, a:b});

# 9.jQuery中如何发起jsonp请求？

1.XHR非跨域请求  $.getJSON('xx.php')

2.跨域请求: $.getJSON('跨域地址/x.php?callback=?',function(data){..处理服务器返回的数据})



	3.$.ajax({
	url:'',

	datatype:'jsonp',

	success:function(data){

         error:function(){}
         }
        }) 







# 10.html5新特性：

1.新的语义标签，2.增强型表单，3.视频和音频，4.Canvas绘图，svg绘图，5.地理定位，6拖放api,7.webWorker  8.webStorage,webSocket

# 11.函数自执行与块级作用域

在ES5中，没有块级作用域，因此我们常常使用函数自执行的方式来模仿块级作用域，这样就提供了一个独立的执行上下文，结合闭包，就为模块化提供了基础。

    (function() {
       // ...
    })();

一个模块往往可以包括：私有变量、私有方法、公有变量、公有方法。

    (function() {
        // 私有变量
        var age = 20;
        var name = 'Tom';


        // 私有方法
        function getName() {
            return `your name is ` + name;
        }


        // 共有方法
        function getAge() {
            return age;
        }

        // 将引用保存在外部执行环境的变量中，形成闭包，防止该执行环境被垃圾回收
        window.getAge = getAge;
    })();




# 12.拖拽的原理

当事件触发时，我们可以通过事件对象获取到鼠标的精切位置。这是实现拖拽的关键。当鼠标按下(mousedown触发)时，我们需要记住鼠标的初始位置与目标元素的初始位置，我们的目标就是实现当鼠标移动时，目标元素也跟着移动，根据常理我们可以得出如下关系：

    移动后的鼠标位置 - 鼠标初始位置 = 移动后的目标元素位置 - 目标元素的初始位置

如果鼠标位置的差值我们用dis来表示，那么目标元素的位置就等于：

    移动后目标元素的位置 = dis + 目标元素的初始位置

通过事件对象，我们可以精确的知道鼠标的当前位置，因此当鼠标拖动(mousemove)时，我们可以不停的计算出鼠标移动的差值，以此来求出目标元素的当前位置。这个过程，就实现了拖拽。

