# SQL 
## 参考资料
[SQL必知必会](https://weread.qq.com/web/reader/f7632a30720befadf7636bb)
[SQL必知必会参考答案](https://forta.com/books/0135182794/challenges/)
[数据库中DML,DDL,DCL,DQL指的是什么意思](https://blog.csdn.net/weixin_45937224/article/details/123079480)

## 基础知识
* DQL
    select
* DML
    insert, delete, update
    insert 通常只能插入一行，要插入多行要么使用多个insert 要么使用 insert select。

    ```sql
    insert into tablename()
    values();
    insert into tablename()
    select * from tablename;
    create table tablename as select * from tableName;
    ```
    <!-- 更新语句 -->
    ```sql 
    update tablename
    set col = colvalue
    where cust_id = 10000000;
    ```
    <!-- 删除语句 -->
    ```sql
    delete from tablename
    where condition;
    ```
* DDL
    create, alter, drop
* 用户管理
  use mysql;
  select user from user;

## 作答
