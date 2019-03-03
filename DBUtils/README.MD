# DBUtils-简化数据库CRUD代码

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
		 // Integer、Long、BigInteger
		Number cnt = (Number)qr.query(sql, new ScalarHandler());		
		long c = cnt.longValue();
		System.out.println(c);
	}
}
```
