# Servlet
  - [Servlet接口](#servlet接口)



## Servlet接口
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
