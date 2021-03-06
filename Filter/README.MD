# 过滤器Filter
  - [过滤器案例](#过滤器案例)

* 过滤器生命周期
```java
public class AFilter implements Filter {
	// 创建之后马上执行，用来做初始化！
	public void init(FilterConfig filterConfig) throws ServletException {
	}

	// 每次过滤时都会执行
	public void doFilter(ServletRequest request, ServletResponse response,
			FilterChain chain) throws IOException, ServletException {
		System.out.println("AFilter#start");
		chain.doFilter(request, response);//放行！
		System.out.println("AFilter#end");
	}

	// 销毁之前执行，用来做对非内存资源进行释放
	public void destroy() {
	}
}
```
* 在web.xml中配置
```java
<filter>
  <filter-name>xxx</filter-name>
  <filter-class>cn.itcast.web.filter.AFitler</fitler-class>
</filter>
<fitler-mapping>
  <filter-name>xxx</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 过滤器案例
  - [案例1](#案例1)
  - [案例2](#案例2)
  - [案例3](#案例3)
  - [案例4](#案例4)
  - [案例5](#案例5)
### 案例1
分ip统计访问次数
```java
public class AListener implements ServletContextListener {
	// 在服务器启动时创建Map，保存到ServletContext
    public void contextInitialized(ServletContextEvent sce) {
    	Map<String,Integer> map = new LinkedHashMap<String,Integer>();
    	// 得到ServletContext
    	ServletContext application = sce.getServletContext();
    	// 把map保存到application中
    	application.setAttribute("map", map);
    }
    public void contextDestroyed(ServletContextEvent sce) {
    }
}
/**
 * 从application中获取Map
 * 从request中得到当前客户端的IP
 * 进行统计工作，结果保存到Map中
 */
public class AFilter implements Filter {
	private FilterConfig config;
	public void destroy() {
	}
	public void doFilter(ServletRequest request, ServletResponse response,
			FilterChain chain) throws IOException, ServletException {
		/*
		 * 1. 得到application中的map
		 * 2. 从request中获取当前客户端的ip地址
		 * 3. 查看map中是否存在这个ip对应访问次数，如果存在，把次数+1再保存回去
		 * 4. 如果不存在这个ip，那么说明是第一次访问本站，设置访问次数为1
		 */
		ServletContext app = config.getServletContext();
		Map<String,Integer> map = (Map<String, Integer>) app.getAttribute("map");

		String ip = request.getRemoteAddr();

		if(map.containsKey(ip)) {//这个ip在map中存在，说明不是第一次访问
			int cnt = map.get(ip);
			map.put(ip, cnt+1);
		} else {//这个ip在map中不存在，说明是第一次访问
			map.put(ip, 1);
		}
		app.setAttribute("map", map);//把map再放回到app中
		
		chain.doFilter(request, response);//肯定放行
	}

	// 在服务器启动时就会执行本方法，而且本方法只执行一次！
	public void init(FilterConfig fConfig) throws ServletException {
		this.config = fConfig;
	}
}
 <body>
<h1 align="center">显示结果</h1>
<table align="center" width="60%" border="1">
	<tr>
		<th>IP</th>
		<th>次数</th>
	</tr>
<c:forEach items="${applicationScope.map }" var="entry">
	<tr>
		<td>${entry.key }</td>
		<td>${entry.value }</td>
	</tr>
</c:forEach>
</table>
  </body>
```
## 案例2
粗粒度权限控制
```java
public class LoginServlet extends HttpServlet {

	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		
		/*
		 * 1. 获取用户名
		 * 2. 判断用户名中是否包含itcast
		 *    如果包含，就是管理员
		 *    如果不包含，就是普通会员
		 * 3. 要把登录的用户名称保存到session中
		 * 4. 转发到index.jsp
		 */
		String username = request.getParameter("username");
		if(username.contains("itcast")) {
			request.getSession().setAttribute("admin", username);
		} else { 
			request.getSession().setAttribute("username", username);
		}
		request.getRequestDispatcher("/index.jsp").forward(request, response);
	}
}
public class UserFilter implements Filter {
	public void destroy() {
	}

	public void doFilter(ServletRequest request, ServletResponse response,
			FilterChain chain) throws IOException, ServletException {
		/*
		 * 1. 得到session
		 * 2. 判断session域中是否存在admin，如果存在，放行；
		 * 3. 否则，判断session域中是否存在username，如果存在，放行，否则打回到login.jsp，并告诉它不要瞎留达
		 */
		HttpServletRequest req = (HttpServletRequest) request;
		String name = (String)req.getSession().getAttribute("admin");
		if(name != null) {
			chain.doFilter(request, response);
			return;
		}
		
		name = (String)req.getSession().getAttribute("username");
		if(name != null) {
			chain.doFilter(request, response);
		} else {
			req.setAttribute("msg", "您啥都不是，不要瞎溜达！");
			req.getRequestDispatcher("/login.jsp").forward(request, response);
		}
	}

	public void init(FilterConfig fConfig) throws ServletException {
	}
}
public class AdminFilter implements Filter {

	public void destroy() {
	}

	public void doFilter(ServletRequest request, ServletResponse response,
			FilterChain chain) throws IOException, ServletException {
		/*
		 * 1. 得到session
		 * 2. 判断session域中是否存在admin，如果存在，放行
		 * 3. 否则打回到login.jsp
		 */
		HttpServletRequest req = (HttpServletRequest) request;
		String name = (String)req.getSession().getAttribute("admin");
		if(name != null) {
			chain.doFilter(request, response);
		} else {
			req.setAttribute("msg", "您可能是个啥，但肯定不是管理员！");
			req.getRequestDispatcher("/login.jsp").forward(request, response);
		}
	}

	public void init(FilterConfig fConfig) throws ServletException {
	}
}
```
## 案例3
```java
public class EncodingFilter implements Filter {
	public void destroy() {
	}
	public void doFilter(ServletRequest request, ServletResponse response,
			FilterChain chain) throws IOException, ServletException {
		// 处理post请求编码问题
		request.setCharacterEncoding("utf-8");
		
		// 处理GET请求的编码问题
		/*
		 * 调包request
		 * 1. 写一个request的装饰类
		 * 2. 在放行时，使用我们自己的request
		 */
		HttpServletRequest req = (HttpServletRequest) request;				
		if(req.getMethod().equals("GET")) {
			EncodingRequest er = new EncodingRequest(req);
			chain.doFilter(er, response);
		} else if(req.getMethod().equals("POST")) {
			chain.doFilter(request, response);
		}
	}

	public void init(FilterConfig fConfig) throws ServletException {
	}
}
// 装饰request
public class EncodingRequest extends HttpServletRequestWrapper {
	private HttpServletRequest req;	
	public EncodingRequest(HttpServletRequest request) {
		super(request);
		this.req = request;
	}
	public String getParameter(String name) {
		String value = req.getParameter(name);		
		// 处理编码问题
		try {
			value = new String(value.getBytes("iso-8859-1"), "utf-8");
		} catch (UnsupportedEncodingException e) {
			throw new RuntimeException(e);
		}	
		return value;
	}
}
```
## 案例4
页面静态化，首先要写一个图书管理项目（省略）
```java
给出一个过滤器，把servlet请求的资源所做的输出保存到html中，重定向到html页面；
二次访问时，这个html已经存在，那么直接重定向，不用再去访问servlet！
public class StaticFilter implements Filter {
	private FilterConfig config;
	public void destroy() {}
	public void init(FilterConfig fConfig) throws ServletException {
		this.config = fConfig;
	}	
	public void doFilter(ServletRequest request, 
			ServletResponse response, FilterChain chain) 
					throws IOException, ServletException {
		HttpServletRequest req = (HttpServletRequest) request;
		HttpServletResponse res = (HttpServletResponse) response;
// 1. 第一次访问时，查找请求对应的html页面是否存在，如果存在重定向到html
// 2. 如果不存在，放行！把servlet访问数据库后，输出给客户端的数据保存到一个html文件中,再重定向到html
		/* 一、获取category参数！
		 * category有四种可能：
		 * * null --> null.html
		 * * 1 --> 1.html
		 * * 2 --> 2.html
		 * * 3 --> 3.html 
		 * html页面的保存路径, htmls目录下
		 * 判断对应的html文件是否存在，如果存在，直接重定向！ */
		String category = request.getParameter("category");
		String htmlPage = category + ".html";//得到对应的文件名称
		String htmlPath = config.getServletContext().getRealPath("/htmls");//得到文件的存放目录
		File destFile = new File(htmlPath, htmlPage);
		
		if(destFile.exists()) {//如果文件存在,重定向到这个文件
			res.sendRedirect(req.getContextPath() + "/htmls/" + htmlPage);
			return;
		}
		
		/* 二、如果html文件不存在，我们要生成html
		 * 放行:show.jsp会做出很多的输出，我们要让它别再输出给客户端，而是输出到指定的一个html文件中
		 * 需要调包response，让它的getWriter()与一个html文件绑定，那么show.jsp的输出就到了html文件中 */
		StaticResponse sr = new StaticResponse(res, destFile.getAbsolutePath());
		chain.doFilter(request, sr);//放行，即生成了html文件
		
		// 这时页面已经存在，重定向到html文件
		res.sendRedirect(req.getContextPath() + "/htmls/" + htmlPage);
	}
}
public class StaticResponse extends HttpServletResponseWrapper {
	private PrintWriter pw;
	// String path：html文件路径！
	public StaticResponse(HttpServletResponse response, String path) 
			throws FileNotFoundException, UnsupportedEncodingException {
		super(response);		
		// 创建一个与html文件路径在一起的流对象
		pw = new PrintWriter(path, "utf-8");
	}

	public PrintWriter getWriter() {
		// 返回一个与html绑定在一起的printWriter对象
		// jsp会使用它进行输出，这样数据都输出到html文件了。
		return pw;
	}
}
```

## 案例5
自动登录（第二次登陆不用再输用户名和密码）
* 登陆界面（略）
* LoginServlet
```java
public class LoginServlet extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		try {
			String userName = request.getParameter("username");
			String password = request.getParameter("password");
			String autoLogin = request.getParameter("auto_login");
			UserBean user = new UserBean();
			user.setUsername(userName);
			user.setPassword(password);	
			UserDao dao = new UserDaoImpl();
			UserBean userBean = dao.login(user);	
			if(userBean != null){		
				//页面提交上来的时候，是否选择了自动登录
				if("on".equals(autoLogin)){			
					//发送cookie给客户端				
					Cookie cookie = new Cookie("auto_login", userName+"#itheima#"+password);
					cookie.setMaxAge(60*60*24*7);//7天有效期
					cookie.setPath("/AutoLoginDemo");
					response.addCookie(cookie);				
				}			
				//成功了，进入首页
				request.getSession().setAttribute("userBean", userBean);
				response.sendRedirect("index.jsp");
			}else{
				//不成功...
				request.getRequestDispatcher("login.jsp").forward(request, response);
			}			
		} catch (SQLException e) {
			e.printStackTrace();
		}}
```
* UserDaoImpl
```java
public class UserDaoImpl implements UserDao {
	@Override
	public UserBean login(UserBean user) throws SQLException {		
		QueryRunner runner = new QueryRunner(JDBCUtil02.getDataSource());
		String sql = "select * from t_user where username = ? and password = ?";
		UserBean query = runner.query(sql, new BeanHandler<UserBean>(UserBean.class) , 
						user.getUsername() , user.getPassword());
		return query;
	}
}
```
* UserBean（略）
* index.jsp
```jsp
<body>
	这是首页 
	<c:if test="${not empty  userBean}">
		欢迎您, ${userBean.username }!
	</c:if>	
	<c:if test="${ empty  userBean}">
		您好，请登录!
	</c:if>	
</body>
```
* AutoLoginFilter
```java
public class AutoLoginFilter implements Filter {
	public void destroy() {  }
	public void doFilter(ServletRequest req, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		try {
			HttpServletRequest request = (HttpServletRequest) req;
			//先判断，现在session中还有没有那个userBean.
			UserBean userBean = (UserBean) request.getSession().getAttribute("userBean");
			//还有，有效。
			if(userBean != null){
				chain.doFilter(request, response);
			}else{
				//代表session失效了。
				//2. 看cookie。
				//1. 来请求的时候，先从请求里面取出cookie , 但是cookie有很多的key-value
				Cookie[] cookies = request.getCookies();
				//2. 从一堆的cookie里面找出我们以前给浏览器发的那个cookie
				Cookie cookie = CookieUtil.findCookie(cookies, "auto_login");
				//第一次来
				if(cookie  == null){
					chain.doFilter(request, response);
				}else{
					//不是第一次。
					String value = cookie.getValue();
					String username = value.split("#itheima#")[0];
					String password = value.split("#itheima#")[1];
					//完成登录
					UserBean user = new UserBean();
					user.setUsername(username);
					user.setPassword(password);
					UserDao dao = new UserDaoImpl();
					userBean = dao.login(user);
					//使用session存这个值到域中，方便下一次未过期前还可以用。
					request.getSession().setAttribute("userBean", userBean);					
					chain.doFilter(request, response);
				}}
		} catch (Exception e) {
			e.printStackTrace();
			chain.doFilter(req, response);
		}}
	public void init(FilterConfig fConfig) throws ServletException {
//		有可能在这里面执行10分钟的耗时
	}}
```
