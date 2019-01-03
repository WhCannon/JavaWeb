# 国际化

```java
public void fun1() {
		Locale locale = Locale.US;
		// 得到ResourceBundle
		// 第一个参数是：基本名称,第二个参数是：Locale
		ResourceBundle rb = ResourceBundle.getBundle("res", locale);
		// 使用ResourceBundle来获取资源信息
		System.out.println(rb.getString("username"));
		System.out.println(rb.getString("password"));
		System.out.println(rb.getString("login"));
	}
```
* jsp中的国际化
```java
<%
 /*
 1. 获取Locale，这是由客户端的浏览器提供的Locale
 2. 创建ResourceBundle
 3. 把所有的语言信息使用rb.getString("xxx")来替换！
 */
 Locale locale = request.getLocale();
 ResourceBundle rb = ResourceBundle.getBundle("res", locale);
 %>
<h1><%=rb.getString("login") %></h1>
<form action="" method="post">
<%=rb.getString("username") %>：<input type="text" name="username"/><br/>
<%=rb.getString("password") %>：<input type="password" name="password"/><br/>
<input type="submit" value="<%=rb.getString("login") %>"/>
```
