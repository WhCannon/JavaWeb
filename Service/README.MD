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
