# 国际化

```java
public void fun1() {
		Locale locale = Locale.US;
		// 得到ResourceBundle
		// 第一个参数是：基本名称,第二个参数是：Locale
		ResourceBundle rb = ResourceBundle.getBundle("res", locale);
		// 使用ResourceBundle来获取资源信息
		System.out.println(rb.getString("username"));
		System.out.println(rb.getString("password"));
		System.out.println(rb.getString("login"));
	}
```
