# JavaBean
  - [BeanUtils](#beanutils)
  
  
  

## BeanUtils
```java
public class Demo1 {
	@Test
	public void fun1() throws Exception {
		String className = "cn.itcast.domain.Person";
		Class clazz = Class.forName(className);
		Object bean = clazz.newInstance();//获取一个对象
		
		BeanUtils.setProperty(bean, "name", "张三");//设置值
		BeanUtils.setProperty(bean, "age", "23");
		BeanUtils.setProperty(bean, "gender", "男");
		BeanUtils.setProperty(bean, "xxx", "XXX");//没有的属性，不会设置
		
		String age = BeanUtils.getProperty(bean, "age");
		System.out.println(age);
		System.out.println(bean);
	}
	
	 // 把map中的属性直接封装到一个bean中　
	 // 把map的数据封装到一个javabean中，必须map的key与bean的属性名相同！
	@Test
	public void fun2() throws Exception {
		Map<String,String> map = new HashMap<String,String>();
		map.put("username", "zhangSan");
		map.put("password", "123");
		
		User user = new User();
		BeanUtils.populate(user, map);//封装进去了
		System.out.println(user);
	}
	
	@Test
	public void fun3() {
		Map<String,String> map = new HashMap<String,String>();
		map.put("username", "zhangSan");
		map.put("password", "123");
		User user = CommonUtils.toBean(map, User.class);
		System.out.println(user);
		// request.getParameterMap();获取表单数据的Map集合，封装到一个类中
	}
}

```
