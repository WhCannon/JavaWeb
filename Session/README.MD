# Session
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
