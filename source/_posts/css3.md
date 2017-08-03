---
title: css3笔记
date: 2016-07-01 15:38:52
tags:
---
# 1、复杂选择器 

# 1、兄弟选择器 
       通过兄弟级别的位置关系来匹配页面元素
        注意，兄弟选择器，只能向后找，不能向前找
        语法：
            相邻兄弟选择器：通过相邻（紧紧挨着的）位置关系匹配元素
            选择器1+选择器2
            如：div+p   #top+.important
                    通用兄弟选择器：用于匹配某元素后面所有的兄弟元素
            选择器1~选择器2
                如#d1~div
# 2、属性选择器
    通过元素所附带的属性及其值来匹配页面中的元素
    语法：
        基础属性选择器  [attr]
            匹配页面中的有附带attr属性的元素
        elem[attr]
            elem:表示任意元素名称
            attr:表示任意属性名称
            匹配页面中附带attr属性的elem元素
            如：div[id]:匹配页面中所有附带id属性的div元素
        [attr1][attr2][attr3]
            匹配页面中同时附带attr1和attr2属性的所有元素
            如 input[name][hype]
        [attr=value]
            匹配页面中所有attr属性的值为value的元素
            如input[type=text]
        [class~=value]
            主要使用在多类选择器上，匹配页面中class属性值中包含value选择器的元素
        [attr^=value]
            匹配以value值作为开始的attr属性的元素
        [attr$=value]
            匹配以value值作为结束的attr属性的元素
        [attr*=value]
            匹配attr属性值中包含value字符的所有元素
# 3、伪类选择器
    目标伪类
        突出显示活动的HTML锚元素
        语法： ：target
    结构伪类
        通过元素之间的结构关系来匹配元素
        ：first-child     获取属于其父元素中的首个子元素
        ：last-child     获取属于其父元素中的尾（最后）子元素
        :nth-child(N)  获取属于其父元素中的第N个子元素
        ：empty   空的，匹配没有子元素的元素，包含文本
        ：only-child   匹配属于其父元素中的唯一子元素
    否定伪类
        把匹配某选择器元素排除出去
        ：not(选择器 )
# 4、伪元素选择器
    伪类与伪元素
        伪类：匹配元素不同的状态
        伪元素：是匹配元素中的内容
    语法：
        ：first-letter
        ::first-letter  匹配某元素的首字符
        ：first-line    匹配某元素的首行字符
        ：：selection 匹配用户选取的内容部分
    ：和：：区别
        在CSS2.1中，伪类选择器和仿元素选择器都是用：来表示
        在CSS3中，所有伪类选择器用：表示，所有的伪元素选择器用：：表示
# 2、内容生成
    通过CSS动态的向某个元素的内容区域之前、之后增加一部分内容
    伪元素选择器
        ：before   定位到元素内容区域之前
        ：after    定位到元素内容区域之后
    语法：
        属性：content
        取值：普通文本
             图像，url(...)
             计数器
    问题处理：
        外边距溢出问题
            为父元素添加边框
            使用父元素的内边距取代子元素的外边距
            在父元素的第一个或最后一个子元素位置处增加一个空的table
        浮动元素父元素的高度问题
# 3、弹性布局
    flexible box,可伸缩布局，为普通布局带来更大的灵活性
    基本概念‘
        flex容器：简称容器，将元素设置为flex容器后，其子元素允许实现灵活的位置摆放
        flex项目：简称项目，存放在flex容器中的内容
    
    语法：
        容器：display
            取值：flex 将块级元素变为flex容器
                  inline-flex  将行内元素变为flex容器
            注意：将元素设置为flex布局后，子元素的float,clear,vertical-align属性将失去作用
        容器属性;
            该组属性要添加在容器元素上，控制子元素的位置
            flex-direction  决定主轴的方向（main-axsis）
                取值：
                    row  主轴为水平方向的轴，起点在容器左端，默认值
                    row-reverse 主轴为水平方向的轴，起点在容器右端
                    column  主轴为交叉轴，起点在容器的顶端
                    column-reverse 主轴为交叉轴，起点在容器的底端
            flex-wrap   当一条轴线（一行）排列不下时，子元素将如何换行
                取值：
                    nowrap     默认值，不换行
                    wrap        换行
                    wrap-reverse  反方向换行
            flex-flow   dirextion和wrap的缩写方式
                取值：
                    row nowrap 默认值
                    direction wrap
            justify-content   定义项目在主轴上的对齐方式
                取值：
                    flex-start   主轴起点对齐
                    flex-end   主轴终点对齐
                    center    居中对齐
                    space-between  两端对齐，项目之间的距离是相等的
                    space-around  每个项目两侧间距是相等的 项目与项目之间的间距，要比项目与父元素之间的间距大一倍
            align-items  定义项目在交叉轴的对齐方式（单行项目有效）
                取值：
                    flex-start  交叉轴起点对齐
                    flex-end   交叉轴终点对齐
                    center     交叉轴中间对齐
                    baseline   基线对齐，以所有项目中的第一行文本为准
                    stretch    默认值，如果项目不设置高度或为auto时，那么项目将占满整个容器的高度
            align-content  定义了多跟轴线的对齐方式，如果项目只一根轴线，该属性无效
                取值：
                    flex-start   交叉轴顶端对齐
                    flex-end    交叉轴底端对齐
                    center      交叉轴中间对齐
                    space-between  与交叉轴两端对齐
                    space-around     项目与项目间对齐
        项目属性：
            该组属性主要设置于项目中
            order  定义项目在排列顺序，值越小，越靠前，默认为0
            flex-grow  指定项目的放大比例，默认为0，即不放大
            flex-shrink 指定项目的缩小比例，默认为1，即空间不足时，等比缩小，为0时不缩小
            flex-basis  指定项目占据主轴的剩余空间大小 auto 默认值，项目本身大小
            flex  是flex-grow,flex-shrink,flex-basis 的简写模式
                取值，auto  相当于1  1   auto
                     none  相当于0 0  auto
                     flex-grow【,flex-shrink,flex-basis】
            align-self  允许定义当个项目与其他项目不一样的交叉轴对齐方式（类似于align-items）,能够覆盖align-items的效果
                取值： auto 默认值，使用
                      flex-start   主轴起点对齐
                    flex-end   主轴终点对齐
                    center    居中对齐
                    base-line
                    stretch
# 4、CSS Hack 兼容性
    标准模式和混杂模式和准标准模式
    IE6之前，没有兼容性说法
    IE6之后，各个浏览器追求标准统一，开始支持标准，IE的其它浏览器要向前兼容，所以出现各种模式
        混杂模式  无标准可言
            编写代码时，不写<!doctylpe>就是混杂模式，采用的是IE5.5的内核进行渲染
        标准模式  安全支持
        准标准模式，即支持标准，也同时向前兼容非标准代码
    如何根据不同的浏览器编写不同的css
        css类内部Hack
            在属性名称前和值添加前后缀以便识别不同的浏览器
        选择器Hack
            在选择器前添加特殊标识以便识别不同的浏览器
        头部引用hack
            通过html的条件注释来判断浏览器版本，去执行不同的CSS
            条件注释
                条件：
                    gt:判断当前浏览器是否大于指定定版本
                    gte：判断当前浏览器是否大于等于指定定版本
                    it:   判断当前浏览器是否小于指定版本
                    ite： 判断当前浏览器是否小于等于指定版本
                    !：   判断当前浏览器是否为非指定版本
                        <!--[if !IE 8]>
                            该段内容在除IE8以外浏览器中显示
                        <![endif]-->
# 5、转换
    
    说明：改变元素在页面中的形状，角度 大小 位置的一种效果
        允许进行2D和3D方向的转换
        2D转换：在平面中进行的操作
        3D转换：在空间中进行的操作
    转换属性：
        rtansform:为元素应用2D或3D转换效果
            取值：none;  没有效果
                transform-functions:一组转换函数
                    位移转换函数：translate()
                    改变形状函数：skew()
                    注意：如果指定多个转换函数的话中间用空格隔开
        转换原点：
            属性：transform-origin
            默认：转换原点在元素中心处
            取值：轴线给值
                两个轴线值：X Y
                三个轴线值：X Y Z
    2D转换
        位移：改变元素在页面中的位置
            语法：transform
                fransform(x)  改变元素在X轴的位置
                fransform(X ,Y)  改变元素在两轴的位置
                fransformX(X) 只在X轴上位置移动
                fransformY(Y)  只在Y轴上位置移动
        缩放： 改变元素在页面中的大小】
            语法：transform
                scale(value)  表示两轴等比缩放
                    取值：默认  为1
                        放大   为大于1的数值
                        缩小   为0~1之间小数
                        返转   负数
                sacle(X,Y)
                saclex(y)
                sacley(y)
        旋转：改变元素在页面上的角度，要根据原点实现转换效果
            语法：transform
                rotate(ndeg)
                    n 取值正，顺时针旋转
                    n 取值负，逆时针旋转
                    deg 为角度
                    0~360范围
            注意：转换原点问题
                元素坐标轴也跟着旋转
        倾斜：改变元素在页面中形状
            语法：transform
                skew(xdeg)  横向倾斜指定度数
                    x 取值正，y轴逆时针倾斜一定角度
                      取值负，Y轴顺时针倾斜一定角度
                skew(xdeg,ydeg)
                skewx(xdeg)
                skewy(ydeg)
    3D转换
        感觉空间
        属性：perspetive 假定人眼到投射平面的距离
        注意：该属性要放在3D转换元素的父元素上
            兼容性chrome和safari需要加前缀
                -wedkit-perspective:500px;
        旋转：以X轴中心轴旋转
                rotatex(xdeg)
              以Y轴中心轴旋转
                rotatey(ydeg)
              以Z轴中心轴旋转
                rotatez(zdeg)
            取值：正  顺时针
                负   逆时针
            以多个轴同时进行旋转
                rotate3d(x ,y, z ,ndeg)
                    x y z 取值为1，该轴参与旋转
                    x y z  取值为0 ，该轴不参与旋转
        位移：改变元素在Z轴上的位置
            语法：transform
                translatez(z)
    
                transform-style
                    取值：flat  默认值，子元素不保留3D位置
                         preserve-3D  子元素保留3D位置
# 6、过渡

    作用效果：使得css属性值在一段时间内平缓变化的效果
    要素与属性：
        指定过渡属性：指定哪个属性值在变化时使用过渡效果展示
            transition-property: 属性名称（width）
                          all   全部属性
                          none
            允许设置过渡效果的属性：
                颜色属性
                渐变属性
                取值为数字属性
                转换属性 transition-property:transform;
                visibility属性
                阴影属性
        指定过渡时长
            transition-duration: 以S、MS为单位数值
        指定过渡时速曲线函数  可选
            transition-timing-function
                取值：ease  默认值，慢速开始，快速变快，慢速结束
                     linear  匀速进行
                     ease-in   慢速开始，快速结束
                     ease-out  快速开始，慢速结束
                     ease-in-out  慢速开始和结束，先加速后减速
        指定过渡的延迟时间   可选
            transition-delay
                取值：以S或MS做为单位
        简写属性：transition:prop duration  timing-fun delay;
            多个过渡效果
                transition:prop1 duration1 timing-fun1 delay1,prop2 duration2 timing-fun2 delay2........;
    触发过渡条件
        将过渡编写在元素声明的样式中，由:hover,:active等，进行触发,
        将过渡编写在:hover,:active伪类中
# 7、动画
    
    使元素从一种样式逐渐变化为另一种样式的过程，与动画相关的属性，增加浏览器前缀
    动画使用步骤
        声明动画
            指定动画名称
            指定动画中的关键帧（keyframes）
                时间点（以百分比描述时间）
                元素状态（CSS样式）
        为元素调用动画
            指定调用动画的名称以及执行时长
    语法：
        声明动画     注意前缀，兼容性问题
            <style>
                @keyframes 名称{
                    0%{   动画开始时，元素的状态   }
                    。。。。
                    100%{  动画结束时，元素的状态  }
                }
            </style>
        调用动画(animation)
            animation-name  指定调用动画名称
            animation-duration   指定动画周期时长，以S或MS为单位
            animation-timing-function  指定动画的速度时间出线函数
                取值：ease  默认值，慢速开始，快速变快，慢速结束
                     linear  匀速进行
                     ease-in   慢速开始，快速结束
                     ease-out  快速开始，慢速结束
                     ease-in-out  慢速开始和结束，先加速后减速
            animation-delay  指定动画延迟时间
            animation-iteration-count  指定动画播放次数
                取值：默认1次，具体数值
                    infinite:无限次播放
            animation-direction  指定动画的播放方向
                取值：normal  从0%~100%
                    reverse  从100%~0%
                    alternate  轮流来回播放 奇数 0%~100%
                                     偶数 100%~0%
            animation  简写方式
                取值：name  duration  timing-fun delay  iteration-count direction;
            animation-fill-mode  指定动画播放之前、之后的填充模式
                取值：none  默认值
                     forwards  动画播放完成后，保持在最后一帧的位置
                     backwards 动画播放前，延迟时间内，动画保持在开始第一帧上的位置
                     both 同时应用在开始和最后的位置帧上
            animation-play-state  动画播放状态
                取值：paused 暂停
                     running 播放
                     
                        