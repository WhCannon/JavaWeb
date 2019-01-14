# IOC BeanFactory
  - [Student](#student)
  - [Teacher](#teacher)
  - [beans.xml](#beans-xml)
  - [StudentDao](#studentdao)
  - [StudentImpl](#studentimpl)
  - [具体使用](#具体使用)
  


## Student
```java
public class Student {
	private String number;
	private String name;
	private int age;
	private String sex;
	private Teacher teacher;
  ...
  }
```

## Teacher
```java
public class Teacher {
	private String tid;
	private String name;
  ...
  }
```

## beans.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<beans>
	<bean id="stu1" className="cn.itcast.domain.Student">
		<property name="number" value="ITCAST_1001"/>
		<property name="name" value="zhangSan"/>
		<property name="age" value="29"/>
		<property name="sex" value="male"/>
		<property name="teacher" ref="t1"/><!-- ref的值必须是另一个been的id -->
	</bean>
	
	<bean id="stu2" className="cn.itcast.domain.Student">
		<property name="number" value="ITCAST_1002"/>
		<property name="name" value="wangWu"/>
		<property name="age" value="94"/>
		<property name="sex" value="female"/>
		<property name="teacher" ref="t1"/><!-- ref的值必须是另一个been的id -->
	</bean>
	
	<bean id="t1" className="cn.itcast.domain.Teacher">
		<property name="tid" value="TEACHER_2001" />
		<property name="name" value="liSi" />
		<property name="salary" value="123.456" />
	</bean>
	
	<bean id="stuDao" className="cn.itcast.dao.impl.StudentImpl">
	</bean>
	
	<bean id="stuService" className="cn.itcast.service.impl.StudentServiceImpl">
		<property name="studentDao" ref="stuDao"/>
	</bean>
</beans>
```

## StudentDao
```java
public interface StudentDao {
	void add(Student stu);
	void update(Student stu);
}
```
## StudentImpl
```java
public class StudentImpl implements StudentDao {
	public void add(Student stu) {
		System.out.println("StudentImpl.add()");
	}
	public void update(Student stu) { 
		System.out.println("StudentImpl.update()");
	}
}
```

## 具体使用
```java
public class Demo1 {
	@Test
	public void fun1() {
		/* 1. 创建Bean工厂，创建时需要给工厂指定配置文件
		 * 2. 从工厂中获取bean对象 */
		BeanFactory bf = new BeanFactory("beans.xml");
		Student s1 = (Student)bf.getBean("stu1");
		Student s2 = (Student)bf.getBean("stu1");
		System.out.println(s1 == s2);//true,说明是单例的
	}
	
	@Test
	public void fun2() {
		/* 1. 创建Bean工厂，创建时需要给工厂指定配置文件
		 * 2. 从工厂中获取bean对象 */
		BeanFactory bf = new BeanFactory("beans.xml");
		Student s1 = (Student)bf.getBean("stu1");
		Student s2= (Student)bf.getBean("stu2");
		System.out.println(s1.getTeacher() == s2.getTeacher());//Teacher也是单例的
	}
	
	@Test
	public void fun3() {
		BeanFactory bf = new BeanFactory("beans.xml");
		StudentDao stuDao = (StudentDao)bf.getBean("stuDao");
		stuDao.add(null);
		stuDao.update(null);
	}
	
	@Test
	public void fun4() {
		BeanFactory bf = new BeanFactory("beans.xml");
		StudentService service = (StudentService) bf.getBean("stuService");
		service.login();
	}
}
```
