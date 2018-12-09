# JavaWeb项目的三层架构
  - [入门-三层HelloWorld](#三层helloworld)
  - [案例：用户注册登录](#用户注册登录)
  
  
## 三层HlloWorld
User.java
```java
// 把数据库中查询出的结果保存到这个对象中。
public class User {
	private String username;
	private String password;
	public User(String username, String password) {
		this.username = username;
		this.password = password;
	}
	public User() {
		super();
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String toString() {
		return "User [username=" + username + ", password=" + password + "]";
	}
}
```
DAO
```java
public class UserDao {
	/*
	 * 把xml中的数据查询出来之后，封装到user对象中，然后返回
	 */
	public User find() {
		return new User("zhangSan", "123"); 
	}
}
```
Service
```java
public class UserService {
	// service层依赖dao层
	private UserDao userDao = new UserDao();
	
	/*
	 * service的查询，需要使用dao来完成！
	 */
	public User find() {
		return userDao.find();
	}
}
```
Servlet
```java
public class UserServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/*
		 * 在servlet中依赖service，然后通过service完成功能，把结果保存到request中
		 * 转发到jsp显示。
		 */
		UserService userService = new UserService();
		User user = userService.find();
		
		request.setAttribute("user", user);
		
		request.getRequestDispatcher("/show.jsp").forward(request, response);
	}
}
```

## 用户注册登录
```java
JSP：
  * login.jsp  --> 登录表单
  * regist.jsp --> 注册表单
  * index.jsp -->  主页(只有登录成功才能看到)

Servlet：
  * LoginServlet
  * RegistServlet

Service：
  * UserService --> 与用户相关的业务类

Dao：
  * UserDao --> 与用户相关的数据类

Domain：
  * User（对应数据库，还要对应所有表单）
    > username
    > password
    > verifyCode
```
* 
