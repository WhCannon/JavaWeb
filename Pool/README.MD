# 数据库连接池
  - [dbcp](#dbcp)
  - [c3p0](#c3p0)
  
## dbcp
```java
public class Demo1 {
	@Test
	public void fun1() throws SQLException {
		// 1. 创建连接池对象
		BasicDataSource dataSource = new BasicDataSource();
		// 2. 配置四大参数
		dataSource.setDriverClassName("com.mysql.jdbc.Driver");
		dataSource.setUrl("jdbc:mysql://localhost:3306/mydb3");
		dataSource.setUsername("root");
		dataSource.setPassword("123");
		
		// 3. 配置池参数
		dataSource.setMaxActive(20);
		dataSource.setMinIdle(3);
		dataSource.setMaxWait(1000);
		
		// 4. 得到连接对象
		Connection con = dataSource.getConnection();
		Connection con1 = new MyConnection(con);
		System.out.println(con1.getClass().getName());
		
		/**
		 * 连接池内部使用四大参数创建了连接对象！即mysql驱动提供的Connection
		 * 连接池使用mysql的连接对象进行了装饰，只对close()方法进行了增强！
		 * 装饰之后的Connection的close()方法，用来把当前连接归还给池！
		 */
		con1.close();//把连接归还给池！
	}
}
```
## c3p0
```java
public class Demo1 {
	@Test
	public void fun1() throws PropertyVetoException, SQLException {
		// 创建连接池对象
		ComboPooledDataSource dataSource = new ComboPooledDataSource();
		
		// 对池进行四大参数的配置
		dataSource.setDriverClass("com.mysql.jdbc.Driver");
		dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/mydb3");
		dataSource.setUser("root");
		dataSource.setPassword("123");
		
		// 配置池参数
		dataSource.setAcquireIncrement(5);
		dataSource.setInitialPoolSize(20);
		dataSource.setMinPoolSize(2);
		dataSource.setMaxPoolSize(50);
		
		Connection con = dataSource.getConnection();
		System.out.println(con);
		con.close();
	}
```
* 配置文件
```java
<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
	<!-- 这是默认配置信息 -->
	<default-config> 
		<!-- 连接四大参数配置 -->
		<property name="jdbcUrl">jdbc:mysql://localhost:3306/mydb3</property>
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="user">root</property>
		<property name="password">123</property>
		<!-- 池参数配置 -->
		<property name="acquireIncrement">3</property>
		<property name="initialPoolSize">10</property>
		<property name="minPoolSize">2</property>
		<property name="maxPoolSize">10</property>
	</default-config>
	
	<!-- 专门为oracle提供的配置信息 -->
	<named-config name="oracle-config"> 
		<property name="jdbcUrl">jdbc:mysql://localhost:3306/mydb1</property>
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="user">root</property>
		<property name="password">123</property>
		<property name="acquireIncrement">3</property>
		<property name="initialPoolSize">10</property>
		<property name="minPoolSize">2</property>
		<property name="maxPoolSize">10</property>
	</named-config>
</c3p0-config>
```
* 通过配置文件来配置参数
```java
public void fun2() throws SQLException{

		// 在创建连接池对象时，这个对象就会自动加载配置文件！不用我们来指定
		ComboPooledDataSource dataSource  = new ComboPooledDataSource();		
		Connection con = dataSource.getConnection();
		System.out.println(con);
		con.close();
	}	
public void fun3() throws SQLException{
		/**
		 * 构造器的参数指定命名配置元素的名称！
		 * <named-config name="oracle-config"> 
		 */
		ComboPooledDataSource dataSource  = new ComboPooledDataSource("oracle-config");
		
		Connection con = dataSource.getConnection();
		System.out.println(con);
		con.close();
	}
```
