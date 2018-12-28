# 事务


## jdbc处理事务的格式
```java
try {
  con.setAutoCommit(false);//开启事务…
  …
  con.commit();//try的最后提交事务
} catch() {
  con.rollback();//回滚事务
}
```

## 转账示例
下面这些代码，所有对Connection的操作都在service层进行的处理！这是不行的！
应该把所有对Connection的操作隐藏起来！这需要使用自定义的小工具！
```java
public class Demo1 {
//转账方法
	public void zhuanZhang(String from, String to, double money) {
		// 对事务的操作必须使用Connection对象!
		Connection con = null;
		try {
			con = JdbcUtils.getConnection();
			//开启事务
			con.setAutoCommit(false);
			AccountDao dao = new AccountDao();
			dao.updateBalance(con, from, -money);//给from减去相应金额
			dao.updateBalance(con, to, money);//给to加上相应金额
			
			//提交事务
			con.commit();
			con.close();
		} catch(Exception e) {
			//回滚事务
			try {
				con.rollback();
				con.close();
			} catch (SQLException e1) {
			}
			throw new RuntimeException(e);
		}
	}
}
public class AccountDao {
	// 修改指定用户的余额！
	public void updateBalance(Connection con, String name, double balance) {
		try {
			 // 给出sql模板，创建pstmt
			String sql = "update account set balance=balance+? where name=?";
			PreparedStatement pstmt = con.prepareStatement(sql);

			 // 对参数进行赋值
			pstmt.setDouble(1, balance);
			pstmt.setString(2, name);

			 // 执行之
			pstmt.executeUpdate();
		} catch(Exception e) {
			throw new RuntimeException(e);
		}
	}
}

```
