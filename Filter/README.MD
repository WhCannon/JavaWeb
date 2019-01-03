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

```


