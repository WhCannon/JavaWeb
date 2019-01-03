# 监听器Listener
  - [编写监听器的步骤](#编写监听器的步骤)
  


## 编写监听器的步骤
* 写一个监听器类：要求必须去实现某个监听器接口
```java
//生命周期监听
public class AListener implements ServletContextListener {
	public void contextInitialized(ServletContextEvent sce) {
		System.out.println("哇，我来也！");
	}
	public void contextDestroyed(ServletContextEvent sce) {
		System.out.println("哇，我要挂也！");
	}
}
//属性监听
public class BListener implements ServletContextAttributeListener {
	public void attributeAdded(ServletContextAttributeEvent scab) {
		System.out.println("您向application中添加了一个名为" + scab.getName() + ", 值为："
				+ scab.getValue() + "的属性");
	}

	public void attributeReplaced(ServletContextAttributeEvent scab) {
		System.out.println(scab.getName() + "=" + scab.getValue() + ", "
				+ scab.getServletContext().getAttribute(scab.getName()));
	}

	public void attributeRemoved(ServletContextAttributeEvent scab) {
		System.out.println(scab.getName() + "=" + scab.getValue());
	}
}
```
* 在web.xml中配置来完成注册
```java
 <listener>
    <listener-class>cn.itcast.web.listener.AListener</listener-class>
  </listener>
```
* 感知监听
```java
public class User implements HttpSessionBindingListener {
	...
	...
	public void valueBound(HttpSessionBindingEvent event) {
		System.out.println("啊~，session添加了我！");
	}
	public void valueUnbound(HttpSessionBindingEvent event) {
		System.out.println("哇哇哇~，无情的session抛弃了我！");
	}
}
```
