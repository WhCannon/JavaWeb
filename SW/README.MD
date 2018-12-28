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
