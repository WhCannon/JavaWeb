# IOC BeanFactory
  - [Student](#student)
  - [Teacher](#teacher)
  - [beans.xml](#beans-xml)
  - [StudentDao](#studentdao)
  


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
	
	<bean id="stuDao" className="cn.itcast.dao.impl.StudentImpl2">
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
