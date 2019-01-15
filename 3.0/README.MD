# Servlet3.0
  - [注解替代配置文件](#注解替代配置文件)
  - [异步处理](#异步处理)
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

## 异步处理
```java
@WebServlet(urlPatterns="/AServlet", asyncSupported=true)
public class AServlet extends HttpServlet {
	public void doGet(final HttpServletRequest req, final HttpServletResponse resp)
			throws ServletException, IOException {
		resp.setContentType("text/html;charset=utf-8");
		
	// 异步处理支持IE！如果输出不足512B，没有异步效果！
	// IE如果不能正常输出，这说明响应体大小不足512B，那你需要多输出点废话！
		for(int i = 0; i <= 512; i++) {
			resp.getWriter().print("a");
		}
		resp.getWriter().flush();
		
		 // 1. 得到异步上下文对象
		final AsyncContext ac = req.startAsync(req, resp);
		
		 // 2. 给上下文对象一个Runnable对象，让它执行这个任务
		ac.start(new Runnable() {
			public void run() {
				println("现在马上开始<br/>", resp);
				sleep(2000);
				for(char c = 'A'; c <= 'Z'; c++) {
					println(c+"", resp);
					sleep(250);
				}				
				// 通知Tomcat我们已经执行结束了！
				ac.complete();
			}
		});
	}
	
	public void println(String text, HttpServletResponse resp) {
		try {
			resp.getWriter().print(text);
			resp.getWriter().flush();
		} catch (IOException e) {
		}
	}
	
	public void sleep(long ms) {
		try {
			Thread.sleep(ms);
		} catch (InterruptedException e) {
		}
	}
}
```
