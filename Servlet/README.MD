# Servlet
  - [Servlet接口](#servlet接口)
  - [ServletConfig](#servletconfig)
  - [GenericServlet](#genericservlet)
  - [ServletContext](#servletcontext)
  - [response](#response)
  - [练习](#练-习)


## Servlet接口
### 接口中的5个方法
```java
public class AServlet implements Servlet {

	//	 它是生命周期方法
	//	 它会在Servlet对象创建之后马上执行，并只执行一次！（出生之后）
	public void init(ServletConfig servletConfig) throws ServletException {
		System.out.println("init()...");
		 // 获取初始化参数
		System.out.println(servletConfig.getInitParameter("p1"));
		System.out.println(servletConfig.getInitParameter("p2"));
		
		 // 获取所有初始化参数的名称
		Enumeration e = servletConfig.getInitParameterNames();
		while(e.hasMoreElements()) {
			System.out.println(e.nextElement());
		}
	}

	 // 它是生命周期方法
	 // 它会被调用多次,每次处理请求都是在调用这个方法！
	public void service(ServletRequest request, ServletResponse response)
			throws ServletException, IOException {
		System.out.println("service()...");
	}

	//是生命周期方法
	//它会在Servlet被销毁之前调用，并且它只会被调用一次！
	public void destroy() {
		System.out.println("destory()...");
	}
	// 获取Servlet的配置信息
	public ServletConfig getServletConfig() {
		System.out.println("getServletConfig()...");
		return null;
	}
	
	//获取Servlet的信息,几乎没用
	public String getServletInfo() {
		System.out.println("getServletInfo()...");
		return "我是一个快乐的Servlet";
	}
}
```

### 浏览器访问Servlet
```xml
<servlet>
		<servlet-name>hello</servlet-name>
		<servlet-class>cn.itcast.servlet.HelloServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>hello</servlet-name>
		<url-pattern>/helloworld</url-pattern>
	</servlet-mapping>  

```

## ServletConfig
```xml
 <servlet>
    <servlet-name>One</servlet-name>
    <servlet-class>cn.itcast.servlet.OneServlet</servlet-class>
    <init-param>
      	<param-name>paramName1</param-name>
    	    <param-value>paramValue1</param-value>
    </init-param>
    <init-param>
    	    <param-name>paramName2</param-name>
    	    <param-value>paramValue2</param-value>
    </init-param>
  </servlet>

```

## GenericServlet
```java
public abstract class GenericServlet implements Servlet, ServletConfig,
        java.io.Serializable {
    private static final long serialVersionUID = 1L;
    private transient ServletConfig config;
    public GenericServlet() {}
    @Override
    public void destroy() {}
    @Override
    public String getInitParameter(String name) {
        return getServletConfig().getInitParameter(name);
    }
    @Override
    public Enumeration<String> getInitParameterNames() {
        return getServletConfig().getInitParameterNames();
    }
    @Override
    public ServletConfig getServletConfig() {
        return config;
    }
    @Override
    public ServletContext getServletContext() {
        return getServletConfig().getServletContext();
    }
    @Override
    public String getServletInfo() {
        return "";
    }
    @Override
    public void init(ServletConfig config) throws ServletException {
        this.config = config;
        this.init();
    }
    public void init() throws ServletException {}
    public void log(String msg) {
        getServletContext().log(getServletName() + ": " + msg);
    }
    public void log(String message, Throwable t) {
        getServletContext().log(getServletName() + ": " + message, t);
    }
    @Override
    public abstract void service(ServletRequest req, ServletResponse res)
            throws ServletException, IOException;
    @Override
    public String getServletName() {
        return config.getServletName();
    }
}

```

## ServletContext
```java
在Servlet中获取ServletContext对象：
public class MyServlet implements Servlet {
public void init(ServletConfig config) {
    ServletContext context = config.getServletContext();
}
…
}
在GenericeServlet或HttpServlet中获取ServletContext对象：
public class MyServlet extends HttpServlet {
public void doGet(HttpServletRequest request, HttpServletResponse response) {
    ServletContext context = this.getServletContext();
}
}
```

* 配置初始化参数
```xml
<web-app ...>
  ...
  <context-param>
	<param-name>paramName1</param-name>
	<param-value>paramValue1</param-value>  	
  </context-param>
 </web-app>

```

## response
* 重定向
```java
public class BServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		System.out.println("BServlet");
		
		response.setHeader("Location", "/day10_1/CServlet");//设置Location
		response.setStatus(302);//发送302状态码
		
		//快捷的重定向方法
		response.sendRedirect("/day10_1/CServlet");
	}
}
```
* 定时刷新（定时重定向）
```java
public class DServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		PrintWriter writer = response.getWriter();
		writer.print("欢迎XXX登录！5秒钟后会自动跳转到主页！您看到的一定是乱码！");
		response.setHeader("Refresh", "5;URL=/day10_1/EServlet");
	}
}
```

## 练 习
### [练习1](#答案1)
统计网站的访问量，一个项目中所有的资源被访问都要对访问量进行累加！

### [练习2](#答案2)




## 答案1
```java
创建一个int类型的变量，用来保存访问量，然后把它保存到ServletContext的域中，这样可以保存所有的Servlet都可以访问到！
最初时，ServletContext中没有保存访问量相关的属性；
当本站第一次被访问时，创建一个变量，设置其值为1；保存到ServletContext中；
当以后的访问时，就可以从ServletContext中获取这个变量，然后在其基础之上加１。

获取ServletContext对象，查看是否存在名为count的属性，
  如果存在，说明不是第一次访问，如果不存在，说明是第一次访问；
第一次访问：调用Servletcontext的setAttribute()传递一个属性，名为count，值为1；
第2~N次访问：调用ServletContext的getAttribute()方法获取原来的访问量，给访问量加1，
            再调用Servletcontext的setAttribute()方法完成设置。

public class AServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		ServletContext app = this.getServletContext();
		Integer count = (Integer)app.getAttribute("count");
		if(count == null) {
			app.setAttribute("count", 1);
		} else {
			app.setAttribute("count", count+1);
		}
		
		//	向浏览器输出 需要使用响应对象！
		PrintWriter pw = response.getWriter();
		pw.print("<h1>" + count + "</h1>");
	}
}
```

## 答案2
```java

```
