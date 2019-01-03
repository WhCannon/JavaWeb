# 过滤器Filter

* 过滤器示例
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
