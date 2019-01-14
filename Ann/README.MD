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

## 反射泛型信息
```java
public class Demo1 {
	@Test
	public void fun1() {
		new B();
	}
}
abstract class A<T> {
	public A() {
		 // 在这里获取子类传递的泛型信息，要得到一个Class！
//		Class clazz = this.getClass();//得到子类的类型
//		Type type = clazz.getGenericSuperclass();//获取传递给父类参数化类型
//		ParameterizedType pType = (ParameterizedType) type;//它就是A<String>
//		Type[] types = pType.getActualTypeArguments();//它就是一个Class数组
//		Class c = (Class)types[0];//它就是String
//		
		Class c = (Class)((ParameterizedType)this.getClass()
				.getGenericSuperclass()).getActualTypeArguments()[0];
		System.out.println(c.getName());
	}
}

class B extends A<String> {
}

class C extends A<Integer> {
}
```
