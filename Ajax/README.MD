# Ajax
  - [Ajax第一例](#Ajax第一例)
  - [校验用户名是否被注册](#例2)
  - [响应内容为xml](#例3)
  

## Ajax第一例
* Servlet
```java
public class AServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		System.out.println("Hello AJAX!");
		response.getWriter().print("Hello AJAX!!!");
	}}
```
* jsp
```jsp
<html>
<head>
...

<script type="text/javascript">
// 创建异步对象，需要先判断浏览器的版本
function createXMLHttpRequest() {
	try {
		return new XMLHttpRequest();//大多数浏览器
	} catch (e) {
		try {
			return ActvieXObject("Msxml2.XMLHTTP");//IE6.0
		} catch (e) {
			try {
				return ActvieXObject("Microsoft.XMLHTTP");//IE5.5及更早版本	
			} catch (e) {
				alert("哥们儿，您用的是什么浏览器啊？");
				throw e;
			}}}}

window.onload = function() {//监听器，文档加载完毕后执行
	var btn = document.getElementById("btn");//获取按钮
	btn.onclick = function() {//给按钮的点击事件注册监听
    //ajax四步操作，得到服务器的响应，把响应结果显示到h1元素中
    
		var xmlHttp = createXMLHttpRequest();//1.得到异步对象
		// 2.打开与服务器的连接
		xmlHttp.open("GET", "<c:url value='/AServlet'/>", true);
		// 3. 发送请求
		xmlHttp.send(null);//GET请求没有请求体，但也要给出null，不然FireFox可能无法发送请求！
		// 4. 给异步对象的onreadystatechange事件注册监听器
		xmlHttp.onreadystatechange = function() {//当xmlHttp的状态发生变化时执行
			// 双重判断：xmlHttp的状态为4（服务器响应结束），以及服务器响应的状态码为200（响应成功）
			if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
				// 获取服务器的响应内容
				var text = xmlHttp.responseText;
				// 获取h1元素
				var h1 = document.getElementById("h1");
				h1.innerHTML = text;
			}};};};
</script>
  </head>
  
  <body>
<button id="btn">点击这里</button>
<h1 id="h1"></h1>
  </body>
</html>
```

## 例2:
注册表单之校验用户是否注册
```java
1. 编写页面：ajax3.jsp
    > 给出注册表单页面
    > 给用户名文本框添加onblur事件的监听
    > 获取文本框的内容，通过ajax发送给服务器，得到响应结果
      * 如果为1：在文本框后显示“用户名已被注册”
      * 如果为0：什么都不做！

2. 编写Servlet: ValidateUsernameServlet
    > 获取客户端传递的用户名参数
    > 判断是否为itcast
      * 是：返回1
      * 否：返回0
```
* jsp
```jsp
<html>
<head>
...

<script type="text/javascript">
判断浏览器版本，省略。。。
window.onload = function() {
	// 获取文本框，给它的失去焦点事件注册监听
	var userEle = document.getElementById("usernameEle");
	userEle.onblur = function() {
		//1.得到异步对象
		var xmlHttp = createXMLHttpRequest();
		//2.打开连接
		xmlHttp.open("POST", "<c:url value='/ValidateUsernameServlet'/>", true);
		//3.设置请求头：Content-Type
		xmlHttp.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
		//4.发送请求，给出请求体
		xmlHttp.send("username=" + userEle.value);
		
		//5.给xmlHttp的onreadystatechange事件注册监听
		xmlHttp.onreadystatechange = function() {
			if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {//双重判断
				//获取服务器的响应，判断是否为1
				// 是：获取span，添加内容：“用户名已被注册”
				var text = xmlHttp.responseText;
				var span = document.getElementById("errorSpan");//得到span元素
				if(text == "1") {
					span.innerHTML = "用户名已被注册！";
				} else {
					span.innerHTML = "";
				}}};};};
</script>
  </head>
  
  <body>
<h1>演示用户名是否被注册</h1>
<form action="" method="post">
用户名：<input type="text" name="username" id="usernameEle"/><span id="errorSpan"></span><br/>
密　码：<input type="password" name="password"/><br/>
<input type="submit" value="注册"/>
</form>
  </body>
</html>
```
* Servlet
```java
public class ValidateUsernameServlet extends HttpServlet {
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		
		/* 1. 获取参数username
		 * 2. 判断是否为itcast
		 *    如果是：响应1
		 *    如果不是：响应0 */
		String username = request.getParameter("username");
		if(username.equalsIgnoreCase("itcast")) {
			response.getWriter().print("1");
		} else {
			response.getWriter().print("0");
		}}}
```
## 例3
响应内容为xml
* Servlet
```java
public class BServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		String xml = "<students>" +
			"<student number='ITCAST_1001'>" +
			"<name>zhangSan</name>" +
			"<age>18</age>" + 
			"<sex>male</sex>" +
			"</student>" +
			"</students>";		
		response.setContentType("text/xml;charset=utf-8");
		response.getWriter().print(xml);
	}
}
```
* jsp
```jsp
<html>
<head>
...

<script type="text/javascript">
判断浏览器版本，省略。。。
window.onload = function() {//文档加载完毕后执行
	var btn = document.getElementById("btn");
	btn.onclick = function() {//给按钮的点击事件注册监听
		var xmlHttp = createXMLHttpRequest();
		xmlHttp.open("GET", "<c:url value='/BServlet'/>", true);
		xmlHttp.send(null);//GET请求没有请求体，但也要给出null，不然FireFox可能会不能发送！
		xmlHttp.onreadystatechange = function() {//当xmlHttp的状态发生变化时执行
			// 双重判断：xmlHttp的状态为4（服务器响应结束），以及服务器响应的状态码为200（响应成功）
			if(xmlHttp.readyState == 4 && xmlHttp.status == 200) {
				// 获取服务器的响应结果（xml）
				var doc = xmlHttp.responseXML;
				// 查询文档下名为student的所有元素，得到数组，再取下标0元素
				var ele = doc.getElementsByTagName("student")[0];
				var number = ele.getAttribute("number");//获取元素名为number的属性值
				var name;
				var age;
				var sex;
				
				// 处理浏览器的差异
				if(window.addEventListener) {
					name = ele.getElementsByTagName("name")[0].textContent;//其他浏览器
				} else {
					name = ele.getElementsByTagName("name")[0].text;//IE支持
				}
				if(window.addEventListener) {
					age = ele.getElementsByTagName("age")[0].textContent;//其他浏览器
				} else {
					age = ele.getElementsByTagName("age")[0].text;//IE支持
				}
				if(window.addEventListener) {
					sex = ele.getElementsByTagName("sex")[0].textContent;//其他浏览器
				} else {
					sex = ele.getElementsByTagName("sex")[0].text;//IE支持
				}

				var text = number + ", " + name + ", " + age + ", " + sex;
				document.getElementById("h1").innerHTML = text;
			}
		};
	};
};
</script>
  </head>
  
  <body>
<button id="btn">点击这里</button>
<h1 id="h1"></h1>
  </body>
</html>
```
