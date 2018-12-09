# JavaWeb项目的三层架构
  - [入门-三层HelloWorld](#三层helloworld)
  
  
## 三层HlloWorld
User.java
```java
// 把数据库中查询出的结果保存到这个对象中。
public class User {
	private String username;
	private String password;
	public User(String username, String password) {
		this.username = username;
		this.password = password;
	}
	public User() {
		super();
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String toString() {
		return "User [username=" + username + ", password=" + password + "]";
	}
}
```
DAO
```java
public class UserDao {
	/*
	 * 把xml中的数据查询出来之后，封装到user对象中，然后返回
	 */
	public User find() {
		return new User("zhangSan", "123"); 
	}
}
```
