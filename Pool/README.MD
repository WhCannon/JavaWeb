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
