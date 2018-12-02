# Servlet
  - [Servlet接口](#servlet接口)
  - [ServletConfig](#servletconfig)
  - [GenericServlet](#genericservlet)


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
