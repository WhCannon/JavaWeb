# MySQL
   - [DDL](#ddl)
   - [DML](#dml)


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
