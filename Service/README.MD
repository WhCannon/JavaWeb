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

Domain实体类：
  * User（对应数据库，还要对应所有表单）
    > username
    > password
    > verifyCode
    
数据库users.xml
    <users>
      <user username="xxx" password="xxx"/>
      <user username="xxx" password="xxx"/>
    </users>
```
* 项目步骤
```java
1. 创建空项目
2. 导包：
  * CommonUtils
  * commons-beanutils.jar
  * commons-logging.jar
  * dom4j
3. 建包
  * cn.itcast.user.domain
    > User
  * cn.itcast.user.dao
    > UserDao
  * cn.itcast.user.service
    > UserService
  * cn.itcast.user.web.servlet
    > LoginServlet
    > RegistServlet
4. jsp
  * login.jsp
  * regist.jsp
  * index.jsp

5. 在F盘下创建一个users.xml文件！
  * 添加根元素<users>
  * 保证文件为utf-8编码！！
```
* 注册
```java
regist.jsp
  > 第一步：完成regist.jsp的基本功能！
RegistServlet
  > 封装表单数据，封装到User对象中。
  > 调用service的regist()方法
    * 如果这个方法没有出问题，输出“注册成功”
    * 如果这个方法抛出了异常，把错误信息保存到request域，转发到regist.jsp(显示错误信息)
UserService#regist()
  > 没有返回值，但注册失败抛出一个自定义的异常！可以在异常中添加异常信息！(自定义一个异常类)
  > 校验用户名是否已被注册（通过用户名查询用户），如果已被注册，抛出异常，异常信息为“用户名已被注册！”
  > 添加用户
UserDao：通过业务分析，得到结果：需要提供两个方法
  > 按用户名查询用户对象：User findByUsername(String username)
  > 插入一个用户到数据库中：void add(User user)

编写过程：
1. 在service层添加一个UserException
2. dao；
  * User findByUsername(String username)
  * void add(User user)
3. service
  * void regist() throws UserException
4. servlet
  1). 封装表单数据到User对象中
  2). 使用user调用service的regist()方法
  3). 如果得到UserException，那么把异常信息保存到request域中，转发回regist.jsp，（数据的回显）
  4). 注册成功，输出“注册成功”
  
给注册添加验证码
1. VerifyCode类
  * BufferedImage getImage() --> 获取随机的验证码图片
  * String getText() --> 获取图片上的文本
  * static output(BfferedImage, OutputStream) --> 把图片写入到指定的输出流中。
2. VerifyCodeServlet
  * 获取随机验证码图片
  * 把验证码图片上的文本保存到session中
  * 把图片响应到response的outputStream中
3. regist.jsp
  * 添加<img src="指向Servlet" />
  * 添加一个文本框，用来输入验证码
  * “看不清，换一张”，是一个超链接。把上面的<img>的src重新再次指向Servlet！为了处理浏览器的缓存，需要使用时间来做参数！
4. 修改RegistServlet
  * 校验验证码！
  * 错误：保存表单数据到request域、保存错误信息到request域，转发回regist.jsp
  * 正确：什么都不做，向下执行原来代码！

登录功能
页面：login.jsp --> 登录表单！
LoginServlet --> 
  1. 获取表单数据，封装到User中
  2. 调用service的login()方法，传递form过去！
  3. 如果service的login()方法，没有抛出异常！返回一个User对象！
  4. 有异常：获取异常信息，保存到request域，保存form，转发到login.jsp
  5. 没异常：保存返回的user对象到session中！！！重定向到welcome.jsp(显示当前用户信息！)
UserService#login()
  public User login(User form) {...}
  1. 使用用户名查询数据库，得到返回的User
    > 返回为null，抛出异常，异常信息为（用户名不存在）
    > 返回不为null，获取查询出来的user的password与form的password进行比较！如果不同：抛出异常（密码错误！）\
    > 如果相同，返回查询结果！

UserDao
  1. 通过用户名查询用户！(已经存在了，不用再写了！)
```
