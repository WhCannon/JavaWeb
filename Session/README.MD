# Cookie&Session
  - [练习](#练-习)
  
  



## 练 习
### [练习1](#答案1)
演示保存用户登录信息（精通）：  
案例相关页面和Servlet：  
login.jsp：登录页面  
succ1.jsp：只有登录成功才能访问的页面  
succ2.jsp：只有登录成功才能访问的页面  
LoginServlet：校验用户是否登录成功！  

## 答案1
```java
各页面和Servlet内容：
login.jsp：提供登录表单，提交表单请求LoginServlet
LoginServlet：获取请求参数，校验用户是否登录成功
    失败：保存错误信息到request域，转发到login.jsp(login.jsp显示request域中的错误信息)
    成功：保存用户信息到session域中，重定向到succ1.jsp页面，显示session域中的用户信息
succ1.jsp：从session域获取用户信息，如果不存在，显示“您还没有登录”。存在则显示用户信息
succ2.jsp：从session域获取用户信息，如果不存在，显示“您还没有登录”。存在则显示用户信息

只要用户没有关闭浏览器，session就一直存在，
那么保存在session中的用户信息也就一起存在！那么用户访问succ1和succ2就会通过！
```
* Login.jsp
```jsp
<body>
    <h1>登录页面</h1>
    <form action="/JavaWeb/LoginServlet" method="post">
    	用户名：<input type="text" name="username"/><br/>
    	密码：<input type="password" name="password"/><br/>
    	<input type="submit" value="登录"/><br/>
    </form>
    <%
    	String msgLog = (String)request.getAttribute("msg");
    	if(msgLog==null){
    		return;
    	}
    %>
    <font color="red" size="8" ><b><%=request.getAttribute("msg") %></b></font>
  </body>
```
* LoginServlet
```java
public class LoginServlet extends HttpServlet {
	 
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		 request.setCharacterEncoding("utf-8");
		 
		 String uname = request.getParameter("username");
		 
		 if("admin".equalsIgnoreCase(uname)){
			 //登录失败
			 request.setAttribute("msg", "用户名或密码错误");
			 RequestDispatcher rq = request.getRequestDispatcher("/SessionTest/Login.jsp");
			 rq.forward(request, response);
		 }else{
			 //登录成功
			 HttpSession session = request.getSession();
			 session.setAttribute("sessionName", uname);
			 response.sendRedirect("/JavaWeb/SessionTest/s1.jsp");
		 }
	}
}
```
* s1.jsp
```jsp
 <body>
    <h1>S1页面</h1>
    <%
    	String sName = (String)session.getAttribute("sessionName");
    	if(sName==null){
    		response.setHeader("Refresh", "5;URL=/JavaWeb/SessionTest/Login.jsp");
    		out.write("<a href=\"/JavaWeb/SessionTest/Login.jsp\">未登录，请返回登录</a>");
    		//request.setAttribute("msg", "未登录，请返回登录");
    		//request.getRequestDispatcher("/SessionTest/Login.jsp").forward(request, response);
    		return;
    		}
    			
    %>
    <font color="blue" size="8"><b><%=sName %>:欢迎登录</b></font>
  </body>
```
