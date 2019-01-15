# 动态代理
  - [例子](#例子)
## 动态代理步骤
```java
public class Demo1 {
	public void fun1() {
		ClassLoader loader = this.getClass().getClassLoader();
		InvocationHandler h = new InvocationHandler() {
			public Object invoke(Object proxy, Method method, Object[] args)
					throws Throwable {
				System.out.println("你好，动态代理！");
				return "xxx";
			}
		};
		// 使用三大参数创建代理对象！！！
		Object o = Proxy.newProxyInstance(loader, new Class[]{A.class, B.class}, h);
		
		// 强转成A和B类型
		A a = (A) o;
		B b = (B) o;
	}
}
interface A {
	public void a();}
interface B {
	public void b();}
```

## 例子
```java
public interface Waiter {
	public void serve();
}
public class ManWaiter implements Waiter {
	public void serve() {
		System.out.println("服务中...");
	}
}

```
