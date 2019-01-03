# 监听器Listener
  - [编写监听器的步骤](#编写监听器的步骤)
  


## 编写监听器的步骤
* 写一个监听器类：要求必须去实现某个监听器接口
```java
public class AListener implements ServletContextListener {
	public void contextInitialized(ServletContextEvent sce) {
		System.out.println("哇，我来也！");
	}
	public void contextDestroyed(ServletContextEvent sce) {
		System.out.println("哇，我要挂也！");
	}
}
```
* 在web.xml中配置来完成注册
```java
 <listener>
    <listener-class>cn.itcast.web.listener.AListener</listener-class>
  </listener>
```
