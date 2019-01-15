# 动态代理
  - [案例](#案例)
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

## 案例
### 案例1
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
### 案例2
代理工厂
```java
/**
 * 它用来生成代理对象
 * 它需要所有的参数
 * * 目标对象
 * * 增强
 * 
 * 1. 创建代理工厂
 * 2. 给工厂设置三样东西：
 *   * 目标对象：setTargetObject(xxx);
 *   * 前置增强：setBeforeAdvice(该接口的实现)
 *   * 后置增强：setAfterAdvice(该接口的实现)
 * 3. 调用createProxy()得到代理对象
 *   * 执行代理对象方法时：
 *   > 执行BeforeAdvice的before()
 *   > 目标对象的目标方法
 *   > 执行AfterAdvice的after()
 */
public class ProxyFactory {
	private Object targetObject;//目标对象
	private BeforeAdvice beforeAdvice;//前置增强
	private AfterAdvice afterAdvice;//后置增强
	getters和setters
	
	// 用来生成代理对象
	public Object createProxy() {
		 // 1. 给出三大参数
		ClassLoader loader = this.getClass().getClassLoader();
		Class[] interfaces = targetObject.getClass().getInterfaces();
		InvocationHandler h = new InvocationHandler() {
			public Object invoke(Object proxy, Method method, Object[] args)
					throws Throwable {
				// 在调用代理对象的方法时会执行这里的内容
				// 执行前置增强
				if(beforeAdvice != null) {
					beforeAdvice.before();
				}
				Object result = method.invoke(targetObject, args);//执行目标对象的目标方法
				// 执行后置增强
				if(afterAdvice != null) {
					afterAdvice.after();
				}
				// 返回目标对象的返回值
				return result;
			}
		};
		 // 2. 得到代理对象
		Object proxyObject = Proxy.newProxyInstance(loader, interfaces, h);
		return proxyObject;
	}
}
```
