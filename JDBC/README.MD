# JDBC
  - [Connection对象](#connection对象)
  - [JDBC完成增删改查](#jdbc完成增删改查)
  - [PreparedStatement](#preparedstatement)
  - [JDBCUtils](#jdbcutils)
  - [处理大型文件](#处理大型文件)
  - [批处理](#批处理)
  - [dbutils](#dbutils)

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
		// 使用url、username、password，建立连接
		Connection con = DriverManager.getConnection(url,username,password);
		
		DriverManager.registerDriver(new com.mysql.jdbc.Driver());注册驱动
		//建立连接
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

## PreparedStatement
```java
public boolean login2(String username, String password) throws Exception {
		/*
		 * 一、得到Connection
		 * 二、得到Statement
		 * 三、得到ResultSet
		 * 四、rs.next()返回的是什么，我们就返回什么
		 */
		// 准备四大参数
		String driverClassName = "com.mysql.jdbc.Driver";
		String url = "jdbc:mysql://localhost:3306/mydb3?useServerPrepStmts=true&cachePrepStmts=true";
		String mysqlUsername = "root";
		String mysqlPassword = "123";
		// 加载驱动类
		Class.forName(driverClassName);
		// 得到Connection
		Connection con = DriverManager.getConnection(url, mysqlUsername, mysqlPassword);
		
		/*
		 * 一、得到PreparedStatement
		 * 1. 给出sql模板：所有的参数使用?来替代
		 * 2. 调用Connection方法，得到PreparedStatement
		 */
		String sql = "select * from t_user where username=? and password=?";
		PreparedStatement pstmt = con.prepareStatement(sql);
		
		// 二、为参数赋值
		pstmt.setString(1, username);//给第1个问号赋值，值为username
		pstmt.setString(2, password);//给第2个问号赋值，值为password
		
		ResultSet rs = pstmt.executeQuery();//调用查询方法，向数据库发送查询语句
		
		pstmt.setString(1, "liSi");
		pstmt.setString(2, "123");
		pstmt.executeQuery();
		return rs.next();
	}
	
	public void fun2() throws Exception {
		String username = "zhangSan";
		String password = "123";
		boolean bool = login2(username, password);
		System.out.println(bool);
	}
	
```

## JDBCUtils
```java
static{
	try {
		//1. 创建一个属性配置对象
		Properties properties = new Properties();
                //使用类加载器，去读取src底下的资源文件。 后面在servlet  //对应文件位于src目录底下
		InputStream is = JDBCUtil.class.getClassLoader().getResourceAsStream("jdbc.properties");
		//导入输入流。
		properties.load(is);
		//读取属性
		driverClass = properties.getProperty("driverClass");
		url = properties.getProperty("url");
		name = properties.getProperty("name");
		password = properties.getProperty("password");				
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
		
		
public class AccountDao {
	public static void update(String name, double money) throws SQLException {
		QueryRunner qr = new TxQueryRunner();
		String sql = "update account set balance=balance+? where name=?";
		Object[] params = {money, name};
		
		// 我们需要自己来提供连接，保证多次调用使用的是同一个连接！
		qr.update(sql, params);
	}
}
/**
 * 这个类中的方法，自己来处理连接的问题，无需外界传递！　
 * 怎么处理的呢？
 *  通过JdbcUtils.getConnection()得到连接！有可能是事务连接，也可能是普通的连接！
 *  JdbcUtils.releaseConnection()完成对连接的释放！如果是普通连接，关闭之！
 */
public class TxQueryRunner extends QueryRunner {
	public int[] batch(String sql, Object[][] params) throws SQLException {
		/*
		 * 1. 得到连接
		 * 2. 执行父类方法，传递连接对象
		 * 3. 释放连接
		 * 4. 返回值
		 */
		Connection con = JdbcUtils.getConnection();
		int[] result = super.batch(con, sql, params);
		JdbcUtils.releaseConnection(con);
		return result;
	}
	public <T> T query(String sql, Object param, ResultSetHandler<T> rsh){...}
	public <T> T query(String sql, Object[] params, ResultSetHandler<T> rsh){...}
	public <T> T query(String sql, ResultSetHandler<T> rsh, Object... params){...}
	public <T> T query(String sql, ResultSetHandler<T> rsh) throws SQLException {...}
	public int update(String sql) throws SQLException {...}
	public int update(String sql, Object param) throws SQLException {...}
	public int update(String sql, Object... params) throws SQLException {...}
	
public class JdbcUtils {
	// 配置文件的默认配置！要求你必须给出c3p0-config.xml！！！
	private static ComboPooledDataSource dataSource = new ComboPooledDataSource();
	
	// 它是事务专用连接！
	private static ThreadLocal<Connection> tl = new ThreadLocal<Connection>();
	
	// 使用连接池返回一个连接对象
	public static Connection getConnection() throws SQLException {
		Connection con = tl.get();
		// 当con不等于null，说明已经调用过beginTransaction()，表示开启了事务！
		if(con != null) return con;
		return dataSource.getConnection();
	}
	
	 // 返回连接池对象！
	public static DataSource getDataSource() {
		return dataSource;
	}
	
	/**
	 * 开启事务
	 * 1. 获取一个Connection，设置它的setAutoComnmit(false)
	 * 2. 还要保证dao中使用的连接是我们刚刚创建的！
	 * --------------
	 * 1. 创建一个Connection，设置它的setAutoComnmit(false)
	 * 2. 把这个Connection给dao用！
	 * 3. 还要让commitTransaction或rollbackTransaction可以获取到！
	 */
	public static void beginTransaction() throws SQLException {
		Connection con = tl.get();
		if(con != null) throw new SQLException("已经开启了事务，就不要重复开启了！");
		// 给con赋值！con设置为手动提交！
		con = getConnection();//给con赋值，表示事务已经开始了
		con.setAutoCommit(false);
		
		tl.set(con);//把当前线程的连接保存起来！
	}
	
	/**
	 * 提交事务
	 * 获取beginTransaction提供的Connection，然后调用commit方法
	 */
	public static void commitTransaction() throws SQLException {
		Connection con = tl.get();//获取当前线程的专用连接
		if(con == null) throw new SQLException("还没有开启事务，不能提交！");
		// 直接使用con.commit()
		con.commit();
		con.close();
		// 把它设置为null，表示事务已经结束了！下次再去调用getConnection()返回的就不是con了
		tl.remove();//从tl中移除连接
	}
	
	/**
	 * 提交事务
	 * 获取beginTransaction提供的Connection，然后调用rollback方法
	 */
	public static void rollbackTransaction() throws SQLException {
		Connection con = tl.get();
		if(con == null) throw new SQLException("还没有开启事务，不能回滚！");
		 // 直接使用con.rollback()
		con.rollback();
		con.close();
		tl.remove();
	}
	
	 // 释放连接　
	public static void releaseConnection(Connection connection) throws SQLException {
		Connection con = tl.get();
		// 判断它是不是事务专用，如果是，就不关闭！
		// 如果不是事务专用，那么就要关闭！
		if(con == null) connection.close();//为null,说明现在没有事务，那么connection一定不是事务专用的！
		// 如果con != null，说明有事务，那么需要判断参数连接是否与con相等，若不等，说明参数连接不是事务专用连接
		if(con != connection) connection.close();
	}
}
```

## 处理大型文件
```java
public class Demo4 {
	/**
	 * 把mp3保存到数据库中。
	 * @throws SQLException 
	 * @throws IOException 
	 * @throws FileNotFoundException 
	 */
	@Test
	public void fun1() throws Exception {
		/*
		 * 1. 得到Connection
		 * 2. 给出sql模板，创建pstmt
		 * 3. 设置sql模板中的参数
		 * 4. 调用pstmt的executeUpdate()执行
		 */
		Connection con = JdbcUtils.getConnection();
		String sql = "insert into tab_bin values(?,?,?)";
		PreparedStatement pstmt = con.prepareStatement(sql);
		
		pstmt.setInt(1, 1);
		pstmt.setString(2, "流光飞舞.mp3");
		/**
		 * 需要得到Blob
		 * 1. 我们有一个文件，目标是Blob
		 * 2. 先把文件变成byte[]
		 * 3. 再使用byte[]创建Blob
		 */
		// 把文件转换成byte[]
		byte[] bytes = IOUtils.toByteArray(new FileInputStream("F:/流光飞舞.mp3"));
		// 使用byte[]创建Blob
		Blob blob = new SerialBlob(bytes);
		// 设置参数
		pstmt.setBlob(3, blob);
		
		pstmt.executeUpdate();
	}

	// 从数据库读取mp3
	@Test
	public void fun2() throws Exception {
		 // 1. 创建Connection
		Connection con = JdbcUtils.getConnection();

		 // 2. 给出select语句模板，创建pstmt
		String sql = "select * from tab_bin";
		PreparedStatement pstmt = con.prepareStatement(sql);
		
		// 3. pstmt执行查询，得到ResultSet
		ResultSet rs = pstmt.executeQuery();
		
		// 4. 获取rs中名为data的列数据
		if(rs.next()) {
			Blob blob = rs.getBlob("data");
			// 把Blob变成硬盘上的文件！
			/*
			 * 1. 通过Blob得到输入流对象
			 * 2. 自己创建输出流对象
			 * 3. 把输入流的数据写入到输出流中
			 */
			InputStream in = blob.getBinaryStream();
			OutputStream out = new FileOutputStream("c:/lgfw.mp3");
			IOUtils.copy(in, out);
		}
	}
}
```

## 批处理
```java
public class Demo5 {
	/**
	 * pstmt对象内部有集合
	 * 1. 用循环疯狂向pstmt中添加sql参数，它自己有模板，使用一组参数与模板就可以匹配出一条sql语句
	 * 2. 调用它的执行批方法，完成向数据库发送！ 
	 */
	public void fun5() throws SQLException {
		Connection con = JdbcUtils.getConnection();
		String sql = "INSERT INTO t_stu VALUES(?,?,?,?)";
		PreparedStatement pstmt = con.prepareStatement(sql);
		
		// 添加参数到批中
		for(int i = 0; i < 10000; i++) {
			pstmt.setInt(1, i+1);
			pstmt.setString(2, "stu_" + i);
			pstmt.setInt(3, i);
			pstmt.setString(4, i%2==0?"男":"女");
			
			pstmt.addBatch();//添加批！这一组参数就保存到集合中了。
		}
		long start = System.currentTimeMillis();
		pstmt.executeBatch();//执行批！
		long end = System.currentTimeMillis();		
		System.out.println(end - start);//412764, 301
	}
}
```

## dbutils
```java
public void fun1() throws SQLException {
		// 对数据库的增、删、改操作
		// 创建QueryRunner，需要提供数据库连接池对象
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "insert into t_stu values(?,?,?,?)";//给出sql模板
		Object[] params = {1002, "liSi", 88, "female"};//给出参数		
		qr.update(sql, params);
	}
	
	@Test
	public void fun2() throws SQLException {
		// 查询数据库
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "select * from t_stu where sid=?";
		Object[] params = {1001};
		
		// 把结果集转换成javabean对象
		// 执行query()方法，需要给出结果集处理器，即ResultSetHandler的实现类对象
		// 我们给的是BeanHandler，它实现了ResultSetHandler
		// 它需要一个类型，然后它会把rs结果集中的数据封装到指定类型的javabean对象中，然后返回javabean
		Stu stu = qr.query(sql, new BeanHandler<Stu>(Stu.class), params);
		System.out.println(stu);
	}
/**
	 * BeanListHandler的应用，它是多行处理器
	 * 每行对象一个Stu对象！
	 * @throws Exception
	 */
	@Test
	public void fun3() throws Exception {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "select * from t_stu";
		List<Stu> stuList = qr.query(sql, new BeanListHandler<Stu>(Stu.class));
		
		System.out.println(stuList);
	}
	
	/**
	 * MapHandler的应用，它是单行处理器，把一行转换成一个Map对象
	 * @throws SQLException 
	 */
	@Test
	public void fun4() throws SQLException  {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "select * from t_stu where sid=?";
		Object[] params = {1001};
		Map map = qr.query(sql, new MapHandler(), params);
		
		System.out.println(map);
	}
	
	/**
	 * MapListHandler，它是多行处理器，把每行都转换成一个Map，即List<Map>
	 * @throws SQLException
	 */
	@Test
	public void fun5() throws SQLException  {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "select * from t_stu";
		List<Map<String,Object>> mapList = qr.query(sql, new MapListHandler());
		
		System.out.println(mapList);
	}
	
	/**
	 * ScalarHandler，它是单行单列时使用，最为合适！
	 * @throws SQLException
	 */
	@Test
	public void fun6() throws SQLException {
		QueryRunner qr = new QueryRunner(JdbcUtils.getDataSource());
		String sql = "select count(*) from t_stu";
		/*
		 * Integer、Long、BigInteger
		 */
		Number cnt = (Number)qr.query(sql, new ScalarHandler());
		
		long c = cnt.longValue();
		System.out.println(c);
	}
}
```
