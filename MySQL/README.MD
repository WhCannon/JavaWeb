# MySQL
   - [DDL](#ddl)
   - [DML](#dml)
   - [约束](#约束)


## DDL
* 对数据库或表的结构进行操作（增删改）
```java
创建表
CREATE TABLE (IF NOT EXISTS) 表名(
    列名 列类型,
    列名 列类型,
    ...
    列名 列类型
  );

CREATE TABLE tb_stu(
      number char(11), 
      name varchar(50), 
      age int, 
      sex varchar(10)
      );
      
修改表
ALTER TABLE 表名 ADD (
      列名 列类型,
      列名 列类型,
      ...
);
```

## DML
* 对表记录进行操作（增删改）
```java

```

## 约束
* 主键约束
```java
CREATE TABLE stu(
	sid	    CHAR(6) PRIMARY KEY,
	sname	VARCHAR(20),
	age		INT,
	gender	VARCHAR(10) 
    );
    指定sid列为主键列，即为sid列添加主键约束
   
    CREATE TABLE stu(
	sid	    CHAR(6),
	sname	VARCHAR(20),
	age		INT,
	gender	VARCHAR(10),
	PRIMARY KEY(sid)
    );
```
* 主键自增长
```java
CREATE TABLE stu(
		sid INT PRIMARY KEY AUTO_INCREMENT,
		sname	VARCHAR(20),
		age		INT,
		gender	VARCHAR(10)
  );
```
* 非空约束
```java
因为某些列不能设置为NULL值，所以可以对列添加非空约束。
  * 例如：
  CREATE TABLE stu(
		sid INT PRIMARY KEY AUTO_INCREMENT,
		sname	VARCHAR(20) NOT NULL,
		age		INT,
		gender	VARCHAR(10)
  );
```
