# 注解

## 定义注解及使用
```java
public @interface MyAnn{}

@MyAnn
public class MyClass {
	@MyAnn
	private int a;
	@MyAnn
	public MyClass() {}
	@MyAnn
	public void fun1() {}
	@MyAnn
	public void fun2(@MyAnn String s) {
		@MyAnn
		int n = 10;
	}
}
```
## 注解的属性
```java
public @interface MyAnn {
	String value();
	int value1();
}

```
