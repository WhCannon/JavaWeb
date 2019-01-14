# Servlet3.0
  - [注解替代配置文件](#注解替代配置文件)
  - []()
  - []()
  

## 注解替代配置文件
```java
@WebServlet(urlPatterns="/AServlet", 
	initParams={
		@WebInitParam(name="p1", value="v1"),
		@WebInitParam(name="p2", value="v2")
	},
	loadOnStartup=1
)
public class AServlet extends HttpServlet {
	public void doGet(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		System.out.println("hello servlet3.0!");
		resp.getWriter().print("hello servlet3.0!!");
	}
}

@WebFilter(urlPatterns="/*")
public class AFilter implements Filter {
	@Override
	public void destroy() {
	}
	@Override
	public void doFilter(ServletRequest request, ServletResponse repsonse,
			FilterChain chain) throws IOException, ServletException {
		System.out.println("哈哈~，你看到我没有！");
		chain.doFilter(request, repsonse);
	}
	@Override
	public void init(FilterConfig arg0) throws ServletException {
		
	}
}

@WebListener
public class AListener implements ServletContextListener {
	@Override
	public void contextDestroyed(ServletContextEvent arg0) {
		System.out.println("死掉了");

	}
	@Override
	public void contextInitialized(ServletContextEvent arg0) {
		System.out.println("出生了");
	}
}
```
