---
title: Javascript实现页面跳转的几种方式
date: 2016-11-21 23:31:35
tags:
---
# 第一种：

# 直接跳转加参数：

    1 <script language="javascript" type="text/javascript">
    2        window.location.href="login.jsp?backurl="+window.location.href; 
    3 </script>
 

# 直接跳转无参数：

    1 <script>window.location.href='http://www.baidu.com';</script>
 

 

# 第二种：返回上一次预览界面

    1 <script language="javascript">
    2 alert("返回");
    3 window.history.back(-1);
    4 </script>
     

# 标签嵌套：
    
    1 <a href="javascript:history.go(-1)">返回上一步</a>
    2 <a href="<%=Request.ServerVariables("HTTP_REFERER")%>">返回上一步</a>
     

 

# 第三种：指定跳转页面 对框架无效。。。

    1 <script language="javascript">
    2        window.navigate("top.jsp");
    3 </script>
 

 

# 第四种：指定自身跳转页面 对框架无效。。。

    1 <script language="JavaScript">
    2           self.location='top.htm';
    3 </script>
 

 

# 第五种：指定自身跳转页面 对框架有效。。。

    1 <script language="javascript">
    2           alert("非法访问！");
    3           top.location='xx.aspx';
    4 </script>
     

 

# 第六种：按钮式 在button按钮添加 事件跳转。。。

    1 <input name="pclog" type="button" value="GO" onClick="location.href='login.aspx'">
     

 

# 第七种：在新窗口打开：

    1 <a href="javascript:" onClick="window.open('login.aspx','','height=500,width=611,scrollbars=yes,status=yes')">开新窗口</a> 
     

 

应用实例：

    
     1 <head> 
     2 <script language="javascript">
     3 
     4 function old_page() 
     5 { 
     6 window.location = "login.aspx" 
     7 } 
     8 function replace() 
     9 { 
    10 window.location.replace("login.aspx") 
    11 } 
    12 function new_page() 
    13 { 
    14 window.open("login.aspx") 
    15 } 
    16 </script> 
    17 </head> 
    18 <body> 
    19 <input type="button" onclick="new_page()" value="在新窗口打开s"/> 
    20 <input type="button" onclick="old_page()" value="跳转后有后退功能"/> 
    21 <input type="button" onclick="replace()" value="跳转后没有后退功能"/> 
    22 </body>