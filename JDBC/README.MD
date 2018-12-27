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
public class Demo2 {
	@Test
	public void fun1() throws ClassNotFoundException, SQLException {
		/*
		 * 得到Connection
		 * 1. 准备四大参数
		 * 2. 加载驱动类
		 * 3. 得到Connection
		 */
		String driverClassName = "com.mysql.jdbc.Driver";
		// jdbc协议的格式！jdbc:工商的名称:子协议（由工商自己来规定）
		// 对mysql而言，它的子协议结构：//主机:端口号/数据库名称
		String url = "jdbc:mysql://localhost:3306/mydb3";
		String username = "root";
		String password = "123";
		
		// 加载驱动类
		Class.forName(driverClassName);
		// 使用DriverManager，以及剩下的3个参数，得到Connection
		Connection con = DriverManager.getConnection(url, username, password);
		
		/*
		 * 对数据库做增、删、改
		 * 1. 通过Connection对象创建Statement，它的功能就是向数据库发送sql语句！
		 * 2. 调用它的int executeUpdate(String sql)，它可以发送DML、DDL
		 */
		// 1. 通过Connection得到Statement对象
		Statement stmt = con.createStatement();
		// 2. 使用Statement发送sql语句！
		String sql = "INSERT INTO stu VALUES('ITCAST_0003', 'wangWu', 88, 'male')";
		String sql2 = "UPDATE stu SET name='zhaoLiu', age=22, " +
				"gender='female' WHERE number='ITCAST_0003'";
		String sql3 = "DELETE FROM stu";
		int r = stmt.executeUpdate(sql);
		System.out.println(r);
	}
	
public void fun3() throws Exception {
		Connection con = null;//定义引用
		Statement stmt = null;
		ResultSet rs = null;
		try {
			/*
			 * 一、得到Connection
			 * 二、得到Statement，发送select语句
			 * 三、对查询返回的“表格”进行解析！
			 */
			String driverClassName = "com.mysql.jdbc.Driver";
			String url = "jdbc:mysql://localhost:3306/exam";
			String username = "root";
			String password = "123";
			Class.forName(driverClassName);
			con = DriverManager.getConnection(url, username, password);//实例化
			
			// 二、得到Statement，执行select语句
			// 1. 得到Statement对象：Connection的createStatement()方法
			// 2. 调用Statement的ResultSet rs = executeQuery(String querySql)
			stmt = con.createStatement();
			String sql = "select * from emp";
			rs = stmt.executeQuery(sql);
			
			// 解析ResultSet
			/*
			 * 三、循环遍历rs，打印其中数据
			 * getString()和getObject()是通用的！
			 */
//			while(rs.next()) {
//				System.out.println(rs.getObject(1) + ", " 
//						+ rs.getString("ename") + ", " + rs.getDouble("sal"));
//			}
			
			int count = rs.getMetaData().getColumnCount();
			while(rs.next()) {//遍历行
				for(int i = 1; i <= count; i++) {//遍历列
					System.out.print(rs.getString(i));
					if(i < count) {
						System.out.print(", ");
					}
				}
				System.out.println();
			}
			
		} catch(Exception e) {
			throw new RuntimeException(e);
		} finally {
			// 关闭资源
			if(rs != null) rs.close();
			if(stmt != null) stmt.close();
			if(con != null) con.close();
		}
		
	}
}
```
