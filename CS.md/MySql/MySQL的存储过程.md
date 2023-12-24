# MySQL的存储过程

# MySQL的存储过程

## 目录

* [MySQL的存储过程](siyuan://blocks/20221215115548-8xbx08x)

  * [目录](siyuan://blocks/20221215115645-2h2i8oh)
  * [什么是存储过程](siyuan://blocks/20221215115350-xk8o86h)
  * [有哪些特性](siyuan://blocks/20221215115350-5cl4xcc)
  * [入门案例](siyuan://blocks/20221215115350-x83mia8)
  * [MySQL 操作 - 变量定义](siyuan://blocks/20221215115350-oorog4u)

    * [局部变量](siyuan://blocks/20221215115350-cpwvfu9)

      * [格式](siyuan://blocks/20221215115350-8kuz524)
      * [操作](siyuan://blocks/20221215115350-rhuklie)
    * [用户变量](siyuan://blocks/20221215115350-726tbmv)

      * [格式](siyuan://blocks/20221215115350-wmc69bs)
      * [操作](siyuan://blocks/20221215115350-nrzt0ls)
    * [系统变量](siyuan://blocks/20221215115350-9eybrao)

      * [介绍](siyuan://blocks/20221215115350-oivq5x3)
    * [系统变量 - 全局变量](siyuan://blocks/20221215115350-ywj2ows)

      * [格式](siyuan://blocks/20221215115350-0x1xcg2)
      * [ 操作](siyuan://blocks/20221215115350-ae1tqlx)
    * [系统变量 - 会话变量](siyuan://blocks/20221215115350-bieuwku)

      * [格式](siyuan://blocks/20221215115350-a5c0hu4)
      * [操作](siyuan://blocks/20221215115350-ztfmsel)
    * [存储过程传参 -in](siyuan://blocks/20221215115350-zr1q907)
    * [存储过程传参 -out](siyuan://blocks/20221215115350-fs1lra5)
    * [存储过程传参 -inout](siyuan://blocks/20221215115350-mavef97)

      * [存储过程传参 -in ， out, inout](siyuan://blocks/20221215115350-tyf10nh)
  * [MySQL 操作 - 流程控制](siyuan://blocks/20221215115350-cb8y0uk)

    * [流程控制 - 判断](siyuan://blocks/20221215115350-df5xnj3)

      * [流程控制 - IF](siyuan://blocks/20221215115350-q7r0v14)

        * [格式](siyuan://blocks/20221215115350-v0ynamp)
        * [操作](siyuan://blocks/20221215115350-i9y64bc)
      * [流程控制 -case](siyuan://blocks/20221215115350-nu6f18s)

        * [格式](siyuan://blocks/20221215115350-zlxlg09)
        * [操作](siyuan://blocks/20221215115350-q1cd1cm)
    * [流程控制 - 循环](siyuan://blocks/20221215115350-zig86ox)

      * [概述：](siyuan://blocks/20221215115350-fot3wew)
      * [循环分类：](siyuan://blocks/20221215115350-ntdbs02)
      * [循环控制：](siyuan://blocks/20221215115350-skn3odm)
      * [流程控制 - 循环 -while](siyuan://blocks/20221215115350-o4d8506)

        * [ 格式](siyuan://blocks/20221215115350-a8644i9)
        * [操作](siyuan://blocks/20221215115350-mozkmg8)
      * [流程控制 - 循环 -repeat](siyuan://blocks/20221215115350-sowy9ws)

        * [格式](siyuan://blocks/20221215115350-aa2mhm2)
        * [操作](siyuan://blocks/20221215115350-zol7o05)
      * [流程控制 - 循环 -loop](siyuan://blocks/20221215115350-617z7dg)

        * [ 格式](siyuan://blocks/20221215115350-0z17bmu)
      * [操作](siyuan://blocks/20221215115350-00qcigz)
    * [ 游标](siyuan://blocks/20221215115350-wgo1tc7)

      * [格式](siyuan://blocks/20221215115350-c99vagj)
      * [操作](siyuan://blocks/20221215115350-w0i5iz6)
    * [异常处理 -HANDLER 句柄](siyuan://blocks/20221215115350-hvo6res)
    * [存储过程中的 handler](siyuan://blocks/20221215115350-8v46y6k)

## 什么是存储过程

* MySQL 5.0 版本开始支持存储过程。

* 简单的说，存储过程就是一组SQL语句集，功能强大，可以实现一些比较复杂的逻辑功能，类似于JAVA语言中的方法；

* 存储过就是数据库 SQL 语言层面的代码封装与重用。

## 有哪些特性

* 有输入输出参数，可以声明变量，有 if/else, case,while 等控制语句，通过编写存储过程，可以实现复杂的逻辑功  
  能；
* 函数的普遍特性：模块化，封装，代码复用；
* 速度快，只有首次执行需经过编译和优化步骤，后续被调用可以直接执行，省去以上步骤；

## 入门案例

```sql
操作 - 创建存储过程
delimiter $$
create procedure proc01()
begin
select empno,ename from emp;
end $$
delimiter ;
-- 调用存储过程
call proc01();
```

## MySQL 操作 - 变量定义


### 局部变量

#### 格式

　　用户自定义，在 begin/end 块中有效

> 语法：声明变量 `codedeclare var_name type [default var_value];`​​  
> 举例： `declare nickname varchar(32);`​​

#### 操作

　　1)

```sql
delimiter $$
create procedure proc02()
begin
declare var_name01 varchar(20) default ‘aaa’;
set var_name01 = ‘zhangsan’;
select var_name01;
end $$
delimiter ;
-- 调用存储过程
call proc02();
```

　　MySQL 中还可以使用 `SELECT..INTO`​​ 语句为变量赋值。其基本语法如下：

```sql
select col_name [...] into var_name[,...]
from table_name wehre condition
--其中：
col_name --参数表示查询的字段名称；
var_name --参数是变量的名称；
table_name --参数指表的名称；
condition --参数指查询条件。
```

　　❗ --注意：当将查询结果赋值给变量时，该查询语句的返回结果只能是**单行单列**。

　　2)

```sql
delimiter $$
create procedure proc03()
begin
declare my_ename varchar(20) ;
select ename into my_ename from emp where empno=1001;
select my_ename;
end $$
delimiter ;
-- 调用存储过程
call proc03();
```

### 用户变量

#### 格式

　　用户自定义，当前会话（连接）有效。类比 java 的成员变量

　　语法：

```sql
@var_name
--不需要提前声明，使用即声明
```

#### 操作

```sql
delimiter $$
create procedure proc04()
begin
set @var_name01 = 'ZS';
end $$
delimiter;
call proc04() ;
select @var_name01 ; -- 可以看到结果
```

### 系统变量

#### 介绍

*  系统变量又分为全局变量与会话变量
*  全局变量在 MYSQL 启动的时候由服务器自动将它们初始化为默认值，这些默认值可以通过更改 my.ini 这个文件来更改。
*  会话变量在每次建立一个新的连接的时候，由 MYSQL 来初始化。 MYSQL 会将当前所有全局变量的值复制一份。来做为会话变量。
*  也就是说，如果在建立会话以后，没有手动更改过会话变量与全局变量的值，那所有这些变量的值都是一样的。
*  全局变量与会话变量的区别就在于，对全局变量的修改会影响到整个服务器，但是对会话变量的修改，只会影响到当前的会话（也就是当前的数据库接）。
*  有些系统变量的值是可以利用语句来动态进行更改的，但是有些系统变量的值却是只读的，对于那些可以更改的系统变量，我们可以利用 set 语句进行更改。

### 系统变量 - 全局变量

　　 由系统提供，在整个数据库有效。  

#### 格式

　　语法：

```sql
@@global.var_name
```

　　‍

####  操作

* 查看全局变量  
  ​`show global variables;`​​

*  查看某全局变量  
  ​`select @@global.auto_increment_increment;`​​-
* 修改全局变量的值
* ​`set global sort_buffer_size = 40000;
  set @@global.sort_buffer_size = 40000;`​​

### 系统变量 - 会话变量

　　由系统提供，当前会话（连接）有效

#### 格式

　　语法：

```sql
@@session.var_name
```

#### 操作

*  查看会话变量  
  ​`show session variables;`​​
* 查看某会话变量  
  ​`select @@session.auto_in`​​crement_increment;
* 修改会话变量的值  
  ​`set session sort_buffer_size = 50000;
  set @@session.sort_buffer_size = 50000 ;`​​

### 存储过程传参 -in

　　in 表示传入的参数， 可以传入数值或者变量，即使传入变量，并不会更改变量的值，可以内部更改，仅仅作用在函数范围内。

```sql
-- 封装有参数的存储过程，传入员工编号，查找员工信息
delimiter $$
create procedure dec_param01(in param_empno varchar(20))
begin
select * from emp where empno = param_empno;
end $$
delimiter ;
call dec_param01('1001');

-- 封装有参数的存储过程，可以通过传入部门名和薪资，查询指定部门，并且薪资大于指定值的员工信息
delimiter $$
create procedure dec_param0x(in dname varchar(50),in sal decimal(7,2),)
begin
select * from dept a, emp b where b.sal > sal and a.dname = dname;
end $$
delimiter ;
call dec_param0x(' 学工部 ',20000);


```

### 存储过程传参 -out

　　out 表示从存储过程内部传值给调用者  

```sql
-- --------- 传出参数： out---------------------------------
use mysql7_procedure;
-- 封装有参数的存储过程，传入员工编号，返回员工名字
delimiter $$
create procedure proc08(in empno int ,out out_ename varchar(50) )
begin
select ename into out_ename from emp where emp.empno = empno;
end $$
delimiter ;
call proc08(1001, @o_ename);
select @o_ename;

-- 封装有参数的存储过程，传入员工编号，返回员工名字和薪资
delimiter $$
create procedure proc09(in empno int ,out out_ename varchar(50) ,out out_sal
decimal(7,2))
begin
select ename,sal into out_ename,out_sal from emp where emp.empno = empno;
end $$
delimiter ;
call proc09(1001, @o_dname,@o_sal);
select @o_dname;
select @o_sal;
```

### 存储过程传参 -inout

　　inout 表示从外部传入的参数经过修改后可以返回的变量，既可以使用传入变量的值也可以修改变量的值（即使函数执行完）

```sql
-- 传入员工名，拼接部门号，传入薪资，求出年薪
delimiter $$
create procedure proc10(inout inout_ename varchar(50),inout inout_sal int)
begin
select concat(deptno,"_",inout_ename) into inout_ename from emp where ename
= inout_ename;
set inout_sal = inout_sal * 12;
end $$
delimiter ;
set @inout_ename = ' 关羽 ';
set @inout_sal = 3000;
call proc10(@inout_ename, @inout_sal) ;
select @inout_ename ;
select @inout_sal ;
```

#### 存储过程传参 -in ， out, inout

*  in 输入参数，意思说你的参数要传到存过过程的过程里面去，在存储过程中修改该参数的  
  值不能被返回
* out 输出参数 : 该值可在存储过程内部被改变，并向外输出
*  inout 输入输出参数，既能输入一个值又能传出来一个值 )

## MySQL 操作 - 流程控制

### 流程控制 - 判断

#### 流程控制 - IF

##### 格式

　　IF 语句包含多个条件判断，根据结果为 TRUE 、 FALSE 执行语句，与编程语言中的 if 、 else if 、 else 语法类似，其语法格式如下：  

```sql
-- 语法
if search_condition_1 then statement_list_1
[elseif search_condition_2 then statement_list_2] ...
[else statement_list_n]
end if
```

##### 操作

```sql
-- 1)输入学生的成绩，来判断成绩的级别：
/*
score < 60 : 不及格
score >= 60 , score <80 : 及格
score >= 80 , score < 90 : 良好
score >= 90 , score <= 100 : 优秀
score > 100 : 成绩错误
*/
delimiter $$
create procedure proc_12_if(in score int)
begin
if score < 60
then
select ' 不及格 ';
elseif score < 80
then
select ' 及格 ' ;
elseif score >= 80 and score < 90
then
select ' 良好 ';
elseif score >= 90 and score <= 100
then
select ' 优秀 ';
else
select ' 成绩错误 ';
end if;
end $$
delimiter ;
call proc_12_if(120)


-- 2)输入员工的名字，判断工资的情况。
delimiter $$
create procedure proc12_if(in in_ename varchar(50))
begin
declare result varchar(20);
declare var_sal decimal(7,2);
select sal into var_sal from emp where ename = in_ename;
if var_sal < 10000
then set result = ' 试用薪资 ';
elseif var_sal < 30000
then set result = ' 转正薪资 ';
else
set result = ' 元老薪资 ';
end if;
select result;
end$$
delimiter ;
call proc12_if(' 庞统 ');
```

#### 流程控制 -case

　　  
CASE 是另一个条件判断的语句，类似于编程语言中的 switch 语法  

##### 格式

```sql
-- 语法一（类比 java 的 switch ）：
case case_value
when when_value then statement_list
[when when_value then statement_list] ...
[else statement_list]
end case
-- 语法二：
case
when search_condition then statement_list
[when search_condition then statement_list] ...
[else statement_list]
end case
```

##### 操作

```sql

-- 语法一
delimiter $$
create procedure proc14_case(in pay_type int)
begin
case pay_type
when 1
then
select ' 微信支付 ' ;
when 2 then select ' 支付宝支付 ' ;
when 3 then select ' 银行卡支付 ';
else select ' 其他方式支付 ';
end case ;
end $$
delimiter ;
call proc14_case(2);
call proc14_case(4);

-- 语法二
delimiter $$
create procedure proc_15_case(in score int)
begin
case
when score < 60
then
select ' 不及格 ';
when score < 80
then
select ' 及格 ' ;
when score >= 80 and score < 90
then
select ' 良好 ';
when score >= 90 and score <= 100
then
select ' 优秀 ';
else
select ' 成绩错误 ';
end case;
end $$
delimiter ;
call proc_15_case(88);
```

### 流程控制 - 循环

　　‍

#### 概述：

* 循环是一段在程序中只出现一次 , 但可能会连续运行多次的代码。
*  循环中的代码会运行特定的次数 , 或者是运行到特定条件成立时结束循环

　　![image](image-20221215115100-p8yu6os.png)

#### 循环分类：

* while
*  repeat
*  loop

#### 循环控制：

* leave 类似于 break ，跳出，结束当前所在的循环
*  iterate 类似于 continue ，继续，结束本次循环，继续下一次

#### 流程控制 - 循环 -while

#####  格式

```sql
【标签 : 】 while 循环条件 do
    循环体 ;
 end while 【 标签】 ;

```

##### 操作

```sql
-- 创建测试表
create table user (
uid int primary_key,
username varchar ( 50 ),
password varchar ( 50 )
);
```

```sql
-- ------- 存储过程 -while
delimiter $$
create procedure proc16_while1(in insertcount int)
begin
declare i int default 1;
label:while i<=insertcount do
insert into user(uid,username,`password`)
values(i,concat('user-',i),'123456');
set i=i+1;
end while label;
end $$
delimiter ;
call proc16_while(10);

-- ------- 存储过程 -while + leave
truncate table user;
delimiter $$
create procedure proc16_while2(in insertcount int)
begin
declare i int default 1;
label:while i<=insertcount do
insert into user(uid,username,`password`)
values(i,concat('user-',i),'123456');
if i=5 then leave label;
end if;
set i=i+1;
end while label;
end $$
delimiter ;
call proc16_while2(10);

-- ------- 存储过程 -while+iterate
朝夕同行
truncate table user;
delimiter $$
create procedure proc16_while3(in insertcount int)
begin
declare i int default 1;
label:while i<=insertcount do
set i=i+1;
if i=5 then iterate label;
end if;
insert into user(uid,username,`password`)
values(i,concat('user-',i),'123456');
end while label;
end $$
delimiter ;
call proc16_while3(10);
```

#### 流程控制 - 循环 -repeat

##### 格式

```sql
[ 标签 :]repeat
循环体 ;
until 条件表达式
end repeat [ 标签 ];
```

##### 操作

```sql
-- ------- 存储过程 - 循环控制 -repeat
use mysql7_procedure;
truncate table user;
delimiter $$
create procedure proc18_repeat(in insertCount int)
begin
declare i int default 1;
label:repeat
insert into user(uid, username, password) values(i,concat('user-',i),'123456');
set i = i + 1;
until i > insertCount
end repeat label;
select ' 循环结束 ';
end $$
delimiter ;
call proc18_repeat(100);
```

#### 流程控制 - 循环 -loop

#####  格式

```sql
[ 标签 :] loop
循环体 ;
if 条件表达式 then
leave [ 标签 ];
end if;
end loop;
```

#### 操作

```sql

-- ------- 存储过程 - 循环控制 -loop
truncate table user;
delimiter $$
create procedure proc19_loop(in insertCount int)
begin
declare i int default 1;
label:loop
insert into user(uid, username, password) values(i,concat('user-',i),'123456');
set i = i + 1;
if i > 5
then
leave label;
end if;
end loop label;
select ' 循环结束 ';
end $$
delimiter ;
call proc19_loop(10);
```

###  游标

　　游标 (cursor) 是用来存储查询结果集的数据类型 , 在存储过程和函数中可以使用光标对结果集进行循环的处理。光标的使用包括光标的声明、 OPEN 、 FETCH 和 CLOSE.  

#### 格式

* 声明语法  
  ​`declare cursor_name cursor for select_statement`​​
* 打开语法  
  ​`open cursor_name`​​
* 取值语法  
  ​`fetch cursor_name into var_name [, var_name] ...`​​
*  关闭语法  
  ​`close cursor_name`​​

#### 操作

```sql
use mysql7_procedure;
delimiter $$
create procedure proc20_cursor(in in_dname varchar(50))
begin
-- 定义局部变量
declare var_empno varchar(50);
declare var_ename varchar(50);
declare var_sal decimal(7,2);
-- 声明游标
declare my_cursor cursor for
select empno , ename, sal
from dept a ,emp b
where a.deptno = b.deptno and a.dname = in_dname;
-- 打开游标
open my_cursor;
-- 通过游标获取每一行数据
label:loop
fetch my_cursor into var_empno, var_ename, var_sal;
select var_empno, var_ename, var_sal;
end loop label;
-- 关闭游标
close my_cursor;
end
朝夕同行
-- 调用存储过程
call proc20_cursor(' 销售部 ');
```

### 异常处理 -HANDLER 句柄

　　MySql 存储过程也提供了对异常处理的功能：通过定义 HANDLER 来完成异常声明的实现 .  
官方文档：https://dev.mysql.com/doc/refman/5.7/en/declare-handler.html

```sql
DECLARE handler_action HANDLER
FOR condition_value [, condition_value] ...
statement
handler_action: {
CONTINUE
| EXIT
| UNDO
}
condition_value: {
mysql_error_code
| condition_name
| SQLWARNING
| NOT FOUND
| SQLEXCEPTION
```

　　❗在语法中，变量声明、游标声明、 handler 声明是必须按照先后顺序书写的，否则创建存储过程出错。

### 存储过程中的 handler

```sql
use mysql7_procedure;
drop procedure if exists proc21_cursor_handler;
-- 需求：输入一个部门名，查询该部门员工的编号、名字、薪资 ，将查询的结果集添加游标
delimiter $$
create procedure proc20_cursor(in in_dname varchar(50))
begin
-- 定义局部变量
declare var_empno int;
declare var_ename varchar(50);
declare var_sal decimal(7,2);
declare flag int default 1; -- ---------------------
-- 声明游标
declare my_cursor cursor for
select empno,ename,sal
from dept a, emp b
where a.deptno = b.deptno and a.dname = in_dname;
-- 定义句柄，当数据未发现时将标记位设置为 0
declare continue handler for NOT FOUND set flag = 0;

--打开游标
open my_cursor;
-- 通过游标获取值
label:loop
fetch my_cursor into var_empno, var_ename,var_sal;
-- 判断标志位
if flag = 1 then
select var_empno, var_ename,var_sal;
else
leave label;
end if;
end loop label;
-- 关闭游标
close my_cursor;
end $$;
call proc21_cursor_handler(' 销售部 ');
```

　　‍
