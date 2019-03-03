# JDBCUtils
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
