# JSP
  - [3种java脚本](#3中java脚本)
  - [Cookie](#cookie)
  - [JSP指令](#jsp指令)
  - [JSP内置对象](#jsp内置对象)
  - [练习](#练-习)



## 3种java脚本
```jsp
<%
   int a = 10;//局部变量
%>
<%
 out.print(a++);//输出局部变量
%>
<br/>
<%=a %> //输出局部变量
<%!
int a = 100;//成员变量
public void fun1() { //定义方法
    System.out.println(a);
}
%>
<%
  out.print(this.a++);//输出成员变量
  fun1(); //调用方法
%>

  
<table border="1" align="center" width="60%">
	<tr>
		<td>姓名</td>
		<td>年龄</td>
	</tr>
<% 
	for(int i = 0; i < 10; i++) {
%>	
	<tr>
		<td>张三</td>
		<td>29</td>
	</tr>
<%
	}
%>
</table>
```

## Cookie
```jsp
 <body>
<h1>保存Cookie</h1>
<%
	Cookie cookie1 = new Cookie("aaa", "AAA");
	response.addCookie(cookie1);
	
	Cookie cookie2 = new Cookie("bbb", "BBB");
	response.addCookie(cookie2);
%>
  </body>
  
<body>
<h1>获取Cookie</h1>
<%
	Cookie[] cookies = request.getCookies();
	if(cookies != null) {
		for(Cookie c : cookies) {
			out.print(c.getName() + "=" + c.getValue() + "<br/>");
		}
	}
%>
  </body>
```

## 练 习
### [练习1](#答案1)
演示jsp与Servlet的分工



## 答案1
* 提交一个表单，看form部分
```jsp
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%
String path = request.getContextPath();
String basePath = request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+path+"/";
%>

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    <title>My JSP 'form.jsp' starting page</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
  </head>
  <body>
<form action="/day11_1/AServlet" method="post">
	整数1：<input type="text" name="num1"/><br/>
	整数2：<input type="text" name="num2"/><br/>
	<input type="submit" value="提交"/>
</form>
  </body>
</html>
```
* Servlet获取参数
```java
public class AServlet extends HttpServlet {
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		// 获取参数
		String s1 = request.getParameter("num1");
		String s2 = request.getParameter("num2");
		// 转换成int类型
		int num1 = Integer.parseInt(s1);
		int num2 = Integer.parseInt(s2);
		
		// 运算
		int sum = num1 + num2;
		
		// 把结果保存到request域中
		request.setAttribute("result", sum);
		
		// 转换到result.jsp
		request.getRequestDispatcher("/jia/result.jsp").forward(request, response);
	}
}
```
* 转发到jsp
```jsp
<body>
<%
   Integer result = (Integer)request.getAttribute("result");
%>
<%=result %>
  </body>
```
