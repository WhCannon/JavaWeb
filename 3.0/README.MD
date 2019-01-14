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
```
