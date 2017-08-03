---
title: DOM，BOM笔记要点
date: 2016-06-06 09:01:30
tags:
---
DOM: Document Object Model

# DOM是专门操作网页内容的API标准
    为什么: 早起js操作不同浏览器的API没有标准，有严重兼容性问题
    所以，W3C制定了统一的操作网页内容的API标准,所有浏览器厂商都遵照执行。
    结果，使用DOM API操作网页内容，几乎100%兼容所有浏览器
    何时: 只要操作网页内容，就用DOM API查找, 修改(内容,属性,样式), 添加, 删除.
    DOM Tree:
    
    什么是: 网页中一切内容在内存中都是以树形结构存储的
           网页中每一项内容都是树上的一个节点对象
           包括: 元素, 文字, 属性...
           树只有一个根节点: document, 包含了所有网页内容
    Node: 每个节点都是一个node类型的对象
          node是所有节点的父类型
# 三大公共: nodeType nodeName nodeValue

    nodeType: 节点的类型
      值: document   9
         element     1
         attribute     2
         text         3
      何时: 只要判断节点类型，就用nodeType
            因为不同类型的节点，能执行的操作是不一样的
      问题: 不能进一步区分元素的名称
      解决:
    nodeName: 节点的名称
      值: document   #document
         element    全大写的标签名
         attribute    属性名
         text        #text
      何时: 只要细致鉴别元素的标签名时
        强调: nodeName返回的是全大写的标签名
    nodeValue: 节点值:
      值: document   null
         element     null
         attribute     属性值
         text         文本内容
# 2、查找: 4种:

      a. 不需要查找，可直接获得的元素
            html   document.documentElement
            head   document.head
            body   document.body
      b. 按节点间关系查找:
    节点树: 包含所有节点: 元素和文本
      1. 父子: elem.parentNode  找elem的父节点
            elem.childNodes  找elem的所有*直接*子节点
                返回，所有直接子节点组成的集合(类数组)
            elem.firstChild   找elem的第一个*直接*子节点
            elem.lastChild   找elem的最后一个*直接*子节点
      2. 兄弟: elem.previousSibling 找elem的前一个兄弟
            elem.nextSibling   找elem的下一个兄弟
     何时: 前提: 已经获得了一个节点
          要找周围临近的节点时
     问题: 连看不见的空字符，也算文本节点——干扰
     解决:
    元素树: 仅包含元素节点的树结构
           不是一棵新树，仅是节点树的子集
     1. 父子: elem.parentElement  找elem的父元素
            elem.children  找elem的所有*直接*子元素
                返回，所有直接子元素组成的集合(类数组)
            elem.firstElementChild   第一个*直接*子元素
            elem.lastElementChild   最后一个*直接*子元素
     2. 兄弟:
       elem.previousElementSibling 找elem的前一个兄弟元素
       elem.nextElementSibling   找elem的下一个兄弟元素
     何时: 只要仅关心元素节点，不关心文本节点时
     问题: IE9+
     强调: childNodes和children返回的都是动态集合！
       凡是遍历动态集合，都要先缓存元素个数，再遍历
       for(var i=0,len= childNodes.length;i<len;i++)
         不会导致反复查找DOM树
# 3、 按HTML查找:

        优: 范围可大可小,可设置条件
    a、按id查找: var elem=document.getElementById("id")
      强调: 1. 只能在document对象上调用
           2. 返回一个元素对象
    b、按标签名查找:
        var elems=parent.getElementsByTagName("标签名");
      强调: 1. 可在任意父元素上
           2. 返回多个元素组成的集合
           3. 不但查找直接子元素，还查找所有后代元素
    c、按name属性查找: 了解
       专门找表单中有name属性的表单元素
        var elems=document.getElementsByName("name")
        强调: 1. 只能在document上调用
             2. 返回多个元素组成的集合
    d、按class属性查找:
        var elems=parent.getElementsByClassName("class")
        强调: 1. 可在任意父元素上调用
             2. 返回多个元素组成的集合
             3. 不要求完整匹配，只要包含即可！
    缺: 每次只能按一个条件查找
       如果条件复杂，就无法一句话获得想要的元素
# 4、 按选择器查找:

    a. 只找一个元素:
      var elem=parent.querySelector("selector");
    b. 找多个元素
      var elems=parent.querySelectorAll("selector");
# 5、 总结:

     A首次查找:
        1. 如果条件简单: 按HTML查找: id, 标签, className
        2. 如果条件复杂: 按选择器查找:
     B已经获得一个元素，找周围相邻: 按节点间关系
    鄙视: 按HTML查找 vs 按选择器查找
     1.使用的难易程度: 当条件复杂时:
        按选择器查找——简单, 按HTML查找——繁琐
     2.返回值:
        getElementsByTagName() 返回多个元素的*动态*集合
          什么是动态集合: 不实际存储对象的属性值，每次访问，都要重新查找DOM树
        querySelectorAll()  返回多个元素的*非动态*集合
          什么是非动态集合: 实际存储对象的所有属性值，即使反复访问集合，也不会导致反复查找DOM树
     3.单次效率:
        按HTML查找——效率高!
        按选择器查找——效率低
# 6、 修改: (内容, 属性, 样式)

    1. 修改:
    标准属性: 2种:
      1. 核心DOM: 操作一切结构化文档的API(HTML，XML)
        elem.attributes集合: 保存了当前元素的所有属性节点
        获取属性值: elem.getAttribute("属性名")
        修改属性值: elem.setAttribute("属性名","值")
        判断是否包含属性: elem.hasAttribute("属性名")
        移除属性: elem.removeAttribute("属性名")
      2. HTML DOM: 对部分常用DOM API的简化版本
         HTML DOM将标准属性都预定义在元素对象中
        获取属性值: elem.属性名
        修改属性值: elem.属性名="值";
        判断是否包含属性: elem.属性名==="" 不包含
        移除属性: elem.属性名=""
        特例: class属性和ES标准中的class重名
              -> DOM -> className
        自定义属性: 比如: data-toggle="dropdown"
          HTML DOM不能操作自定义属性
          暂时只能用核心DOM操作:
        三大状态: disabled  selected   checked
          核心DOM无法操作三大状态属性
          HTMLDOM: elem.disabled elem.selected  elem.checked
                值都是bool类型true/false
# 6.1、修改css样式:

    1. 仅获取/修改内联样式:  elem.style.css属性名
      问题1: css属性名有的带-
      解决: 所有css属性名都要去横线变驼峰
         比如: background-color: backgroundColor
              list-style-type: listStyleType
      问题2: 所有数值类型的属性值都是带单位的字符串
      解决: 获取时: 都要去单位，转数值
            修改时: 将单位拼回数值
      问题3: 仅能获得内联样式, 无法获得样式表中的样式
      解决: 计算后的样式: 最终应用到元素上的完整样式
        何时: 只要希望获得元素完整的样式时
        如何: 2步:
          1. 获得完整样式对象style
            var style=getComputedStyle(elem)
          2. 获得style对象中的css属性
            style.css属性名
         强调: style对象中的样式都是只读
    结论: 1. 获取样式: getComputedStyle
         2. 修改样式: elem.style.css属性名
    2. 运行时修改样式表中的样式:
      Step1: 获得样式表对象:
       var sheet=document.styleSheets[i]
      Step2: 获得样式表对象中某个CSSRule(一个选择器{})
       var rule=sheet.cssRules[i]
      Step3: 修改rule.style.css属性名=值
# 7、 添加和删除:

    添加: 3步:
     Step1: 创建空元素:
      var a=document.createElement("a");
      <a></a>
     Step2: 设置关键属性:
         a.href="http://tmooc.cn"
         a.innerHTML="go to tmooc";
      <a href="http://tmooc.cn">go to tmooc</a>
     Step3: 将元素添加到DOM树: 3种:
       1. 末尾追加: parent.appendChild(child)
       2. 中间插入: parent.insertBefore(child, oldChild)
       3. 替换: parent.replaceChild(child, oldChild)
# 优化: 尽量少的修改DOM树

    原因: 页面加载过程:
      html -> DOM Tree(松树)
               ↓
            render Tree(圣诞树)-> layout(计算绝对布局)->paint
               ↑                 最耗时
      css  -> cssRules(装饰品)
      每次修改DOM树，都会导致重新layout，耗时。
    如何: 2种:
     1. 如果同时添加父元素和子元素时，应该先在内存将子元素都添加到父元素中，再将父元素一次性整体添加到DOM树
        结果: 只触发一次layout
# 1、 HTML DOM 常用对象: 对常用HTML元素操作的简化

    Select: 代表页面上的一个select元素
     属性: select.value 当前选中项的value
                     没有value，就返回选中项的内容
          select.options 保存select下所有option元素对象
            相当于: select.getElementsByTagName("option")
            select.options.length 保存select下option的个数
            清空select下所有option: select.options.length=0;
          select.length 等效于select.options.length
            清空select下所有option: select.length=0;
                                   select.innerHTML="";
          select.selectedIndex 当前选中项的下标
      事件: onchange 当选中项发生改变时
      方法: select.add(option) 向select中添加一个option
             相当于: select.appendChild(option)
             不支持文档片段
           select.remove(i) 移除select中i位置的一个option
    Option: 代表页面上的一个option元素
      创建: var opt=new Option(text,value);
         创建一个option对象，同时设置opt的内容为text，设置opt的值为value
         相当于: var opt=document.createElement("option");
                opt.innerHTML=text;
                opt.value=value;
      属性: .text 代替.innerHTML
           .index  表示当前option在select下的下标位置
# Table: 代表网页中一个table元素

     管着行分组：
       添加行分组: var 行分组=table.createTHead|TBody|TFoot();
           强调: 即创建，同时又将行分组添加到table
       删除行分组: table.deleteTHead|TFoot()
       获取行分组: table.tHead|tFoot
                  table.tBodies[i]
    行分组: THead TBody TFoot
      管着行:
       添加行: var tr=行分组.insertRow(i)
           在行分组中i位置插入一个新行
           强调: 中间插入行，原i位置的行向后顺移
           固定套路: 1. 末尾追加一个新行: 行分组.insertRow()
                    2. 开头插入: 行分组.insertRow(0)
       删除行: 行分组.deleteRow(i)
           删除行分组中第i行
           强调: i是当前行在行分组内的相对下标位置
       获取行: 行分组.rows
    
    行: tr
      管着td:
        添加td: var td=tr.insertCell(i);
            省略i表示右侧末尾追加
            insertCell不支持添加th，只能添加td
        删除td: tr.deleteCell(i);
        获取td: tr.cells
    
    删除行:
     tr上都有一个属性: tr.rowIndex 行在整个表的绝对下标
     问题：行分组，无法使用tr.rowIndex删除行。
     解决: table.deleteRow(tr.rowIndex)
     总结: 今后，删除行都用table.deleteRow(tr.rowIndex)
# form: 代表页面上一个表单元素

     获取: var form=document.forms[i/id]
     属性: form.elements 保存了表单中所有表单元素的数组
            包括: input   select   textarea  button
          form.elements.length 获得表单中表单元素的个数
          form.length => form.elements.length
     方法: form.submit();  用于手动提交表单
     事件: form.onsubmit  以任何方式提交表单之前自动触发
              常用于在提交之前，验证所有表单元素的内容
    表单元素:
     获取: var elem=form.elements[i/id/name]
            简写: 如果表单元素有name属性: form.name
     方法: elem.focus() 让elem获得焦点
          elem.blur()  让elem失去焦点
    
    Image: 代表页面上一个img元素
      创建: var img=new Image();
# DOM总结: 查找->绑定事件->查找->修改/添加/删除

    查找: 4种:
      1. 不需要查找可直接获得: html  head  body  form
      2. 节点间关系: 节点树/元素树
          鄙视: 递归遍历
      3. 按HTML: 4种: ById, ByTagName, ByName, ByClassName
      4. 按选择器: 2种:
          只找一个: querySelector()
          找多个: querySelectorAll()
    修改:
      内容: .innerHTML  .textContent/.innerText  .value
      属性:
        1. 标准属性: 1. 核心DOM; 2. HTML DOM
        2. 自定义属性: 核心DOM
        3. 状态属性: HTML DOM
      样式:
        修改: elem.style.css属性=值
        获取: var style=getComputedStyle(elem)
             style.css属性 ——只读
        可通过修改class属性批量应用修改多个css属性
    添加: 3步:
       1. createElement,
       2.设置关键属性,
       3. appendChild/insertBefore/replaceChild
      优化: 尽量少的操作DOM树
      如何: 2种:
       1. 同时添加父子元素: 先将子元素加入父元素，再将父元素整体添加到页面
       2. 同时添加多个平级子元素: fragment
    删除: parent.removeChild(child)
    HTML DOM: Select/Option  Table/...  From/Element  Image
    过渡动画: 2步:
      css中: 添加transition
      js中: 修改css属性值
       不支持transition: display  zIndex
       支持: width  height  opacity   bottom/top/left/right ...
# 2、BOM: Browser Object Model

    什么是: 专门操作浏览器窗口的API
    比如: alert prompt confirm
    问题: 1. 没有标准——兼容性问题;
         2. 不可定制
    window对象: 2个角色:
      1. 代替ES中的Global充当全局作用域对象
      2. 封装所有BOM和DOM的API
    
    打开超链接: 4种:
      1. 在当前窗口打开，可后退
        html: <a href="url" target="_self"></a>
        js: /*window.*/open("url","_self")
      2. 在当前窗口打开，不可后退
        js: location.replace("url");
           用新url代替history中当前url，结果: 无法后退
      3. 在新窗口打开，可打开多个
        html: <a href="url" target="_blank"></a>
        js: open("url","_blank")
      4. 在新窗口打开，只能打开一个
        html: <a href="url" target="自定义name属性值"></a>
        js: open("url","自定义name属性值")
        原理: 内存中每个窗口都有一个唯一的name属性来唯一标示一个窗口
          浏览器规定，相同name属性的窗口只能打开一个
        其实: html中的target属性就是在设置新窗口的name属性值。
        如果target中使用自定义的窗口名，则只能打开一个
        预定义:
          _self: 默认使用当前窗口自己的name属性
               结果，新窗口覆盖当前窗口
          _blank: 意为不指定窗口名, 浏览器会随机生成不同的窗口名。
               结果: 每次打开新窗口都随机生成不同的name
                     结果: 可打开任意多个
# 定时器: 2种:

    1. 周期性定时器:
      什么是: 让程序按照指定时间间隔，反复执行一项任务
      何时: 只要让程序按照指定时间间隔，反复执行一项任务
      如何: 3件事:
        1. 任务函数: 让定时器反复调用的函数
        2. 启动定时器:
         var timer=setInterval(任务函数, 间隔的毫秒数)
        3. 停止定时器: clearInterval(timer)
            问题: timer中的序号会残留在timer变量中
            解决: 停止定时器后，主动清空timer
                 timer=null
      停止定时器: 2种:
        1. 用户手动停止定时器: 用按钮调用clearInterval
        2. 自动停止定时器: 在任务函数中:
           1. 设定临界条件
           2. 如果达到临界条件就自动调用clearInterval
    
    2. 一次性定时器:
     什么是: 让程序先等待一段时间，再自动执行一次任务
             执行一次后，定时器自动停止
     何时: 只要先等待，再执行一次任务
     如何: 三件事
       1. 任务函数
       2. 启动: var timer=setTimeout(任务函数, 等待的毫秒数)
       3. 停止: clearTimeout(timer)
    鄙视: 定时器中的函数，只能在主程序所有程序执行后才能执行
    
    for(var i=0;i<3;i++){
      setTimeout(function(){
        console.log(i);
      },0);
    }//结果: 3 3 3
    //alert("Hello") 如果不点确定，则永远不输出333
# window:
# history，location，document，navigator，screen，event 

    history: 保存当前窗口打开后，成功访问过的历史记录的栈
      history封装的非常严密
      只能前进，后退，刷新: history.go(n)
       前进: go(1)  后退:go(-1)  刷新:go(0)
    
    location: 专门保存当前窗口正在打开的url的对象
     属性: location.href 保存了完整的url
            在当前窗口打开: location.href=新url
          location.protocol: 协议
                .host: 主机名+端口号
                .hostname: 主机名
                .port: 端口号
          location.pathname: 相对路径
                .hash: 锚点地址#xxx
                .search: 表单提交后地址栏中的查询字符串
                       ?变量名=值&变量名=值&...
     方法:
       1. 替换history中当前url,实现进制后退:
         location.replace("新url")
       2. 在当前页面打开，可后退:
         location.assign("新url")
           => location.href="新url"
            => location="新url"
       3. 刷新页面:  location.reload(false/true);
         鄙视: false/true的差别
           浏览器本地是有缓存的
             浏览器的缓存中会保存css，图片等静态资源
           每次请求时，首先查看缓存中是否有想要文件
             没有想要文件，或文件过期，才去服务器下载新文件
           reload(false) 优先使用本地缓存的文件
           reload(true) 强制去服务器下载新文件
         查 浏览器缓存的原理！
# 1、event

    绑定事件: 2种:
     1. 在HTML中绑定: <ANY on事件名="js语句"
        问题: 不符合内容与行为分离的原则——不便于维护
     2. 在js中动态绑定: 2种:
        1. 一个事件只绑定一个处理函数:
           elem.on事件名=function(){
             //this->elem
           }
           解除绑定: elem.on事件名=null;
           问题: 每个事件只能绑定一个处理函数
           解决:
        2. 一个事件可同时绑定多个处理函数:
           elem.addEventListener("事件名",function(){
             //this->elem
           })
           解除绑定:
            elem.removeEventListener("事件名","函数名");
            强调: 如果一个事件处理函数可能被动态移除，则绑定时，不能使用匿名函数，必须使用有名称的函数
    
        事件模型: DOM标准: 3个阶段
          1. 捕获: 由外向内，记录各级父元素绑定的事件处理函数
          2. 目标触发: 首先执行目标元素上的事件处理函数
          3. 冒泡: 由内向外，反向执行捕获阶段记录的处理函数
    
        事件对象: 事件发生时自动创建的
                 封装事件信息
                 提供操作事件的API 的对象
          何时: 只要希望获得事件信息或修改事件的默认行为
          如何: 事件对象，在事件发生时，通常作为事件处理函数的第一个参数，默认自动传入！
              .on事件名=function(e){
                 //e会自动获得事件对象
              }
        阻止蔓延/冒泡: e.stopPropagation();
        利用冒泡:
          优化: 尽量少的添加事件监听
          原理: 因为浏览器触发事件监听，是采用遍历查找的方式。添加的监听越多，遍历的速度越慢
          如何: 如果多个子元素都要绑定相同的事件
              只要在父元素绑定一次，所有子元素即可共用
          难题:
             1. 获得目标元素:
                不能用this, 因为this指父元素
                应该用e.target，保存实际点击的目标元素
             2. 鉴别目标元素:
                先判断目标元素的nodeName或className...
                只有目标元素符合要求时，才执行事件操作
        取消事件/阻止默认行为: e.preventDefault();
        事件坐标: 3对儿:
          1. 相对于整个屏幕左上角的坐标: e.screenX|screenY
          2. 相对于文档显示区左上角的坐标: e.clientX|clientY
          3. 相对于当前元素左上角的坐标: e.offsetX|offsetY
    
        页面滚动:
          事件: window.onscroll
          获得页面滚动位置: document.body.scrollTop
              页面超出文档显示区顶部的距离