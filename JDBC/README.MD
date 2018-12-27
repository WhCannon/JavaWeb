# JDBC
  - [Connection对象](#connection对象)
  - [JDBC完成增删改查](#jdbc完成增删改查)


## Connection对象
```java
public void fun1() throws ClassNotFoundException, SQLException {
		/*
		 * jdbc四大配置参数：
		 * > driverClassName：com.mysql.jdbc.Driver
		 * > url：jdbc:mysql://localhost:3306/数据库名
		 * > username：root
		 * > password：123
		 */

		Class.forName("com.mysql.jdbc.Driver");// 加载驱动类(注册驱动)
		String url="jdbc:jdbc:mysql://localhost:3306/mydb3";
		String username="root";
		String password="123";
		// 使用url、username、password，得到连接对象
		Connection con = DriverManager.getConnection(url,username,password);
		
		Class.forName("com.mysql.jdbc.Driver");// 加载驱动类(注册驱动)
		com.mysql.jdbc.Driver driver = new com.mysql.jdbc.Driver();
		DriverManager.registerDriver(driver);
		Connection con = DriverManager.getConnection(
				"jdbc:mysql://localhost:3306/mydb3", "root", "123");
		System.out.println(con);
	}
}
```

## JDBC完成增删改查
```java

```
