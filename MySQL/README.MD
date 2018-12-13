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
* 唯一约束
```java
CREATE TABLE stu(
		sid INT PRIMARY KEY AUTO_INCREMENT,
		sname	VARCHAR(20) NOT NULL UNIQUE,
		age		INT,
		gender	VARCHAR(10)
  );
```
* 外键约束
```java
CREATE TABLE emp (
    empno INT PRIMARY KEY,
    ...
    deptno INT,
    CONSTRAINT fk_emp FOREIGN KEY(mgr) REFERENCES emp(empno)  
  );
  
修改表时添加外键约束
  ALERT TABLE emp
  ADD CONSTRAINT fk_emp_deptno FOREIGN KEY(deptno) REFERENCES dept(deptno);

修改表时删除外键约束
  ALTER TABLE emp
  DROP FOREIGN KEY fk_emp_deptno;/*约束名称*/
```
* 多对多关系
```java
create table student(
    sid int PRIMARY KEY,
    ...
  );
  create table teacher(
    tid int PRIMARY KEY,
    ...
  );

  create table stu_tea(
    sid int,
    tid int,
    ADD CONSTRAINT fk_stu_tea_sid FOREIGN KEY(sid) REFERENCES student(sid),
    ADD CONSTRAINT fk_stu_tea_tid FOREIGN KEY(tid) REFERENCES teacher(tid)
  );
  这时在stu_tea这个中间表中的每条记录都是来说明student和teacher表的关系
  例如在stu_tea表中的记录：sid为1001，tid为2001，这说明编号为1001的学生有一个编号为2001的老师
```
