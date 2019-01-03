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


