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
public class Demo2 {
	public void fun1() {
		Waiter manWaiter = new ManWaiter();//目标对象
		 // 给出三个参数，来创建方法，得到代理对象
		ClassLoader loader = this.getClass().getClassLoader();
		Class[] interfaces = {Waiter.class};
		InvocationHandler h = new WaiterInvocationHandler(manWaiter);//参数manWaiter表示目标对象
		// 得到代理对象，代理对象就是在目标对象的基础上进行了增强的对象！
		Waiter waiterProxy = (Waiter)Proxy.newProxyInstance(loader, interfaces, h);
		waiterProxy.serve();//前面添加“您好”，　后面添加“再见”
	}
}
class WaiterInvocationHandler implements InvocationHandler {
	private Waiter waiter;//目标对象
	public WaiterInvocationHandler(Waiter waiter) {
		this.waiter = waiter;
	}
	public Object invoke(Object proxy, Method method, Object[] args)
			throws Throwable {
		System.out.println("您好！");
		this.waiter.serve();//调用目标对象的目标方法
		System.out.println("再见！");
		return null;
	}
}
```
