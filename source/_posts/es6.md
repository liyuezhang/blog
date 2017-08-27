---
title: ES6 变量作用域与提升详解
date: 2017-02-25 20:22:43
tags:
---
# 变量作用域与提升

在 ES6 之前，JavaScript 中只存在着函数作用域；而在 ES6 中，JavaScript 引入了 let、const 等变量声明关键字与块级作用域，在不同作用域下变量与函数的提升表现也是不一致的。在 JavaScript 中，所有绑定的声明会在控制流到达它们出现的作用域时被初始化；这里的作用域其实就是所谓的执行上下文（Execution Context），每个执行上下文分为内存分配（Memory Creation Phase）与执行（Execution）这两个阶段。在执行上下文的内存分配阶段会进行变量创建，即开始进入了变量的生命周期；变量的生命周期包含了声明（Declaration phase）、初始化（Initialization phase）与赋值（Assignment phase）过程这三个过程。

传统的 var 关键字声明的变量允许在声明之前使用，此时该变量被赋值为 undefined；而函数作用域中声明的函数同样可以在声明前使用，其函数体也被提升到了头部。这种特性表现也就是所谓的提升（Hoisting）；虽然在 ES6 中以 let 与 const 关键字声明的变量同样会在作用域头部被初始化，不过这些变量仅允许在实际声明之后使用。在作用域头部与变量实际声明处之间的区域就称为所谓的暂时死域（Temporal Dead Zone），TDZ 能够避免传统的提升引发的潜在问题。另一方面，由于 ES6 引入了块级作用域，在块级作用域中声明的函数会被提升到该作用域头部，即允许在实际声明前使用；而在部分实现中该函数同时被提升到了所处函数作用域的头部，不过此时被赋值为 undefined。

# 作用域

    作用域（Scope）即代码执行过程中的变量、函数或者对象的可访问区域，作用域决定了变量或者其他资源的可见性；计算机安全中一条基本原则即是用户只应该访问他们需要的资源，而作用域就是在编程中遵循该原则来保证代码的安全性。除此之外，作用域还能够帮助我们提升代码性能、追踪错误并且修复它们。JavaScript 中的作用域主要分为全局作用域（Global Scope）与局部作用域（Local Scope）两大类，在 ES5 中定义在函数内的变量即是属于某个局部作用域，而定义在函数外的变量即是属于全局作用域。

# 全局作用域

当我们在浏览器控制台或者 Node.js 交互终端中开始编写 JavaScript 时，即进入了所谓的全局作用域：

// the scope is by default global
var name = 'Hammad';
定义在全局作用域中的变量能够被任意的其他作用域中访问：

    var name = 'Hammad';
    
    console.log(name); // logs 'Hammad'
    
    function logName() {
        console.log(name); // 'name' is accessible here and everywhere else
    }
    
    logName(); // logs 'Hammad'
# 函数作用域

定义在某个函数内的变量即从属于当前函数作用域，在每次函数调用中都会创建出新的上下文；换言之，我们可以在不同的函数中定义同名变量，这些变量会被绑定到各自的函数作用域中：
    
    // Global Scope
    function someFunction() {
        // Local Scope #1
    function someOtherFunction() {
            // Local Scope #2
        }
    }
    
    // Global Scope
    function anotherFunction() {
        // Local Scope #3
    }
    // Global Scope
函数作用域的缺陷在于粒度过大，在使用闭包或者其他特性时导致异常的变量传递：

    var callbacks = [];
    
    // 这里的 i 被提升到了当前函数作用域头部
    for (var i = 0; i <= 2; i++) {
        callbacks[i] = function () {
    return i * 2;
            };
    }
    
    console.log(callbacks[0]()); //6
    console.log(callbacks[1]()); //6
    console.log(callbacks[2]()); //6
# 块级作用域

类似于 if、switch 条件选择或者 for、while 这样的循环体即是所谓的块级作用域；在 ES5 中，要实现块级作用域，即需要在原来的函数作用域上包裹一层，即在需要限制变量提升的地方手动设置一个变量来替代原来的全局变量，譬如：

    var callbacks = [];
    for (var i = 0; i <= 2; i++) {
        (function (i) {
            // 这里的 i 仅归属于该函数作用域
            callbacks[i] = function () {
    return i * 2;
            };
        })(i);
    }
    callbacks[0]() === 0;
    callbacks[1]() === 2;
    callbacks[2]() === 4;
而在 ES6 中，可以直接利用 let 关键字达成这一点：

    let callbacks = []
    for (let i = 0; i <= 2; i++) {
        // 这里的 i 属于当前块作用域
        callbacks[i] = function () {
            return i * 2
        }
    }
    callbacks[0]() === 0
    callbacks[1]() === 2
    callbacks[2]() === 4
# 词法作用域

词法作用域是 JavaScript 闭包特性的重要保证，笔者在基于 JSX 的动态数据绑定一文中也介绍了如何利用词法作用域的特性来实现动态数据绑定。一般来说，在编程语言里我们常见的变量作用域就是词法作用域与动态作用域（Dynamic Scope），绝大部分的编程语言都是使用的词法作用域。词法作用域注重的是所谓的 Write-Time，即编程时的上下文，而动态作用域以及常见的 this 的用法，都是 Run-Time，即运行时上下文。词法作用域关注的是函数在何处被定义，而动态作用域关注的是函数在何处被调用。JavaScript 是典型的词法作用域的语言，即一个符号参照到语境中符号名字出现的地方，局部变量缺省有着词法作用域。此二者的对比可以参考如下这个例子：

    function foo() {
        console.log( a ); // 2 in Lexical Scope ，But 3 in Dynamic Scope
    }
    
    function bar() {
    var a = 3;
        foo();
    }
    
    var a = 2;
    
    bar();
# 执行上下文与提升

作用域（Scope）与上下文（Context）常常被用来描述相同的概念，不过上下文更多的关注于代码中 this 的使用，而作用域则与变量的可见性相关；而 JavaScript 规范中的执行上下文（Execution Context）其实描述的是变量的作用域。众所周知，JavaScript 是单线程语言，同时刻仅有单任务在执行，而其他任务则会被压入执行上下文队列中（更多知识可以阅读 Event Loop 机制详解与实践应用）；每次函数调用时都会创建出新的上下文，并将其添加到执行上下文队列中。


# 变量的生命周期与提升

变量的生命周期包含着变量声明（Declaration Phase）、变量初始化（Initialization Phase）以及变量赋值（Assignment Phase）三个步骤；其中声明步骤会在作用域中注册变量，初始化步骤负责为变量分配内存并且创建作用域绑定，此时变量会被初始化为 undefined，最后的分配步骤则会将开发者指定的值分配给该变量。


传统的 var 关键字声明的变量会被提升到作用域头部，并被赋值为 undefined：

    // var hoisting
    num;     // => undefined  
    var num;  
    num = 10;  
    num;     // => 10  
    // function hoisting
    getPi;   // => function getPi() {...}  
    getPi(); // => 3.14  
    function getPi() {  
    return 3.14;
    }
变量提升只对 var 命令声明的变量有效，如果一个变量不是用 var 命令声明的，就不会发生变量提升。

    console.log(b);
    b = 1;
上面的语句将会报错，提示 ReferenceError: b is not defined，即变量 b 未声明，这是因为 b 不是用 var 命令声明的，JavaScript 引擎不会将其提升，而只是视为对顶层对象的 b 属性的赋值。ES6 引入了块级作用域，块级作用域中使用 let 声明的变量同样会被提升，只不过不允许在实际声明语句前使用：

    > let x = x;
    ReferenceError: x is not defined
        at repl:1:9
        at ContextifyScript.Script.runInThisContext (vm.js:44:33)
        at REPLServer.defaultEval (repl.js:239:29)
        at bound (domain.js:301:14)
        at REPLServer.runBound [as eval] (domain.js:314:12)
        at REPLServer.onLine (repl.js:433:10)
        at emitOne (events.js:120:20)
        at REPLServer.emit (events.js:210:7)
        at REPLServer.Interface._onLine (readline.js:278:10)
        at REPLServer.Interface._line (readline.js:625:8)
    > let x = 1;
    SyntaxError: Identifier 'x' has already been declared
# 函数的生命周期与提升

基础的函数提升同样会将声明提升至作用域头部，不过不同于变量提升，函数同样会将其函数体定义提升至头部；譬如：

    function b() {  
       a = 10;  
    return;  
    function a() {} 
    } 
    会被编译器修改为如下模式：
    
    function b() {
    function a() {}
      a = 10;
    return;
    }
在内存创建步骤中，JavaScript 解释器会通过 function 关键字识别出函数声明并且将其提升至头部；函数的生命周期则比较简单，声明、初始化与赋值三个步骤都被提升到了作用域头部：

如果我们在作用域中重复地声明同名函数，则会由后者覆盖前者：

    sayHello();
    
    function sayHello () {
    function hello () {
            console.log('Hello!');
        }
    
        hello();
    
    function hello () {
            console.log('Hey!');
        }
    }
    
    // Hey!
而 JavaScript 中提供了两种函数的创建方式，函数声明（Function Declaration）与函数表达式（Function Expression）；函数声明即是以 function 关键字开始，跟随者函数名与函数体。而函数表达式则是先声明函数名，然后赋值匿名函数给它；典型的函数表达式如下所示：
    
    var sayHello = function() {
      console.log('Hello!');
    };
    
    sayHello();
    
    // Hello!
函数表达式遵循变量提升的规则，函数体并不会被提升至作用域头部：

    sayHello();
    
    function sayHello () {
    function hello () {
            console.log('Hello!');
        }
    
        hello();
    
    var hello = function () {
            console.log('Hey!');
        }
    }
    
    // Hello!
在 ES5 中，是不允许在块级作用域中创建函数的；而 ES6 中允许在块级作用域中创建函数，块级作用域中创建的函数同样会被提升至当前块级作用域头部与函数作用域头部。不同的是函数体并不会再被提升至函数作用域头部，而仅会被提升到块级作用域头部：

    f; // Uncaught ReferenceError: f is not defined
    (function () {
      f; // undefined
      x; // Uncaught ReferenceError: x is not defined
    if (true) {
        f();
        let x;
    function f() { console.log('I am function!'); }
      }
    
    }());
# 避免全局变量

在计算机编程中，全局变量指的是在所有作用域中都能访问的变量。全局变量是一种不好的实践，因为它会导致一些问题，比如一个已经存在的方法和全局变量的覆盖，当我们不知道变量在哪里被定义的时候，代码就变得很难理解和维护了。在 ES6 中可以利用 let关键字来声明本地变量，好的 JavaScript 代码就是没有定义全局变量的。在 JavaScript 中，我们有时候会无意间创建出全局变量，即如果我们在使用某个变量之前忘了进行声明操作，那么该变量会被自动认为是全局变量，譬如:

    function sayHello(){
      hello = "Hello World";
    return hello;
    }
    sayHello();
    console.log(hello);
在上述代码中因为我们在使用 sayHello 函数的时候并没有声明 hello 变量，因此其会创建作为某个全局变量。如果我们想要避免这种偶然创建全局变量的错误，可以通过强制使用 strict mode 来禁止创建全局变量。

# 函数包裹

为了避免全局变量，第一件事情就是要确保所有的代码都被包在函数中。最简单的办法就是把所有的代码都直接放到一个函数中去:

    (function(win) {
        "use strict"; // 进一步避免创建全局变量
    var doc = window.document;
        // 在这里声明你的变量
        // 一些其他的代码
    }(window));