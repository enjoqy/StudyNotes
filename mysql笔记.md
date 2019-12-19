# mysql

```java
/*    一、数据库
*     1.定义：Database简称DB，web项目中的DataSource，数据的仓库，实现数据的持久化和管理。
*     2.分类：
*        早期分类分为三种层次式数据库、网络式数据库、关系型数据库；
*        近期分类分为两种，关系型数据库和非关系型数据库(NOSQL：Not Only SQL);
*           1.关系型数据库：用二维表描述复杂的结构化数据
*              MySQL，Oracle、SQLServer
*           2.非关系型数据库：是传统关系型数据库的补充
*              1.键值存储形式：Redis
*              2.列存储形式：HBase
*              3.文档存储形式：MongoDB
*  二、MySQL数据库
*     1.服务的启动与关闭
*        net start mysql
*        net stop mysql
*     2.登录数据库
*        mysql -hlocalhost -P3306 -uroot -proot
*     3.退出数据库
*        exit、quit
*     4.目录
*        数据库的安装目录：C:\Program Files\MySQL\MySQL Server 5.7
*        数据库的文件目录：C:\ProgramData\MySQL\MySQL Server 5.7\Data
*  三、SQL
*     1.定义：Structured Query Language结构化查询语言，定义了一套数据库操作语言规范，
*           适用于绝大部分数据库。每种数据库具体操作有所区别，被称为数据库的“方言”。
*     2.通用语法：
*        1.SQL语句不区分大小写，关键字建议使用大写。
*        2.单行多行都可以，每一条语句以分号结尾。
*        3.注释：
*           1.单行注释：-- 注释内容
*           2.多行注释：/ * 注释内容 * /
*           3.mysql的单行注释：  # 注释内容
*     3.SQL语句分类
*        1.DDL：数据定义语言
*           操作对象：数据库、表
*           关键字：create、alter、drop、show...
*        2.DML：数据操作语言
*           操作对象：数据、记录、元组
*           关键字：insert、update、delete
*        3.DQL：数据查询语言
*           操作对象：数据、记录、元组
*           关键字：select、from、where...
*        4.DCL：数据控制语言
*           操作对象：数据库、用户
*           关键字：grant、remoke...
*        5.TCL：事务控制语言
*           操作对象：数据库
*           关键字：commit、rollback、savepoint
* 
* 
*  注：“与数据库无关”、“数据库无关性”
*/
```

## 1、概述

###   **1.1、显示数据库**

```mysql
SHOW DATABASES;
```

### **1.2、创建数据库**

```mysql
  # utf-8
  CREATE DATABASE 数据库名称 DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
 
  # gbk
  CREATE DATABASE 数据库名称 DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;
  
  # 简单格式
  create database 数据库名称；
```

### **1.3、打开数据库**

```
  USE db_name;
  注：每次使用数据库必须打开相应数据库
```

###   **1.4、用户管理**

## 2、数据库和表内容的操作(增、删、改、查)

###  1、增

```mysql
insert into 表 (列名,列名...) values (值,值,值...)
insert into 表 (列名,列名...) values (值,值,值...),(值,值,值...)
insert into 表 (列名,列名...) select (列名,列名...) from 表
```

```mysql
ALTER TABLE 表名 ADD 列 列的类型;
```



### 2、删

```mysql
delete from 表
delete from 表 where id＝1 and name＝'alex'
```

```mysql
 ALTER TABLE 表名  DROP i;
```



### 3、改

```mysql
update 表 set name ＝ 'alex' where id>1
```

```mysql
ALTER TABLE 表 CHANGE 旧字段 新字段 新字段类型;
```



### 4、查

#### 4.1、普通查询

```mysql
select * from 表
select * from 表 where id > 1
select nid,name,gender as gg from 表 where id > 1
```

```mysql
a、条件
    select * from 表 where id > 1 and name != 'alex' and num = 12;
 
    select * from 表 where id between 5 and 16;
 
    select * from 表 where id in (11,22,33)
    select * from 表 where id not in (11,22,33)
    select * from 表 where id in (select nid from 表)
b、限制
    select * from 表 limit 5;            - 前5行
    select * from 表 limit 4,5;          - 从第4行开始的5行
    select * from 表 limit 5 offset 4    - 从第4行开始的5行

更多选项查询
```

####  4.2、数据排序(查询)

```mysql
排序
    select * from 表 order by 列 asc              - 根据 “列” 从小到大排列
    select * from 表 order by 列 desc             - 根据 “列” 从大到小排列
    select * from 表 order by 列1 desc,列2 asc    - 根据 “列1” 从大到小排列，如果相同则按列2从小到大排序
```

####     4.3、模糊查询

```mysql
通配符(模糊查询)
    select * from 表 where name like 'ale%'  - ale开头的所有（多个字符串）
    select * from 表 where name like 'ale_'  - ale开头的所有（一个字符）
```

#### 4.4、聚集函数查询

```mysql
1）COUNT
        语法：COUNT(e1)
        参数：e1为一个表达式，可以是任意的数据类型
        返回：返回数值型数据
        作用：返回e1指定列不为空的记录总数

2）SUM,
        语法：SUM(e1)
        参数：e1为类型为数值型的表达式
        返回：返回数值型数据
        作用：对e1指定的列进行求和计算

3）MIN, MAX
        语法：MIN(e1)、MAX(e1)
        参数：e1为一个字符型、日期型或数值类型的表达式。
            若e1为字符型，则根据ASCII码来判断最大值与最小值。
        返回：根据e1参数的类型，返回对应类型的数据。
        作用：MIN(e1)返回e1表达式指定的列中最小值；
              MAX(e1)返回e1表达式指定的列中最大值；

4）AVG
        语法：AVG(e1)
        参数：e1为一个数值类型的表达式
        返回：返回一个数值类型数据
        作用：对e1表达式指定的列，求平均值。

5）MEDIAN
        语法：MEDIAN(e1)
        参数：e1为一个数值或日期类型的表达式
        返回：返回一个数值或日期类型的数据
        作用：首先，根据e1表达式指定的列，对值进行排序；
            若排序后，总记录为奇数，则返回排序队列中，位于中间的值；
            若排序后，总记录为偶数，则对位于排序队列中，中间两个值进行求平均，返回这个平均值；
6）RANK
        1）用法1:RANK OVER
               语法：    RANK( )  OVER ([ PARTITION BY column1 ] ORDER BY column2 [ASC|DESC])
                为分析函数，为每条记录产生一个序列号，并返回。
               参数：    column1为列名，指定按照哪一列进行分类（分组）
                  column2为列名，指定根据哪列排序，默认为升序；
                  若指定了分类子句（PARTITION BY），则对每类进行排序（每个分类单独排序）
               返回：返回一个数值类型数据，作为该记录的序号！
               作用：为分析函数，对记录先按column1分类，再对每个分类进行排序，并为每条记录分配一个序号（每个分类单独排序）
               注意：排序字段值相同的记录，分配相同的序号。存在序号不连续的情况    
               实例：student表记录了学生每科的成绩，要求按学科排序，并获取每科分数前两名的记录
                student表如下：        
                SQL> select * from student order by kemu;
                 NAME       ID                KEMU      FENSHU
                ---------- -------------- -------------- ----------------
                Li            0113101     物理               80
                Luo         0113011     物理               80
                Wang     0113077     物理               70
                Zhang     0113098    物理               90
                Luo         0113011     高数               80
                Wang      0113077    高数               70
                Zhang     0113098    高数               80
                Li             0113101    高数               90
rows selected
                按学科分类，按成绩排序（降序）
                SQL> select rank() over(partition by KEMU order by FENSHU desc) as sort,student.* from student;
                       SORT    NAME        ID              KEMU      FENSHU
                ---------- ---------- ---------------- ------------ ----------
           Zhang      0113098    物理               90
           Li              0113101    物理               80
           Luo           0113011    物理               80
           Wang       0113077    物理               70
           Li              0113101    高数               90
           Luo           0113011    高数               80
           Zhang      0113098    高数               80
           Wang       0113077    高数               70
                由返回记录可了解，对排序列的值相同的记录，rank为其分配了相同的序号（SORT NAME列）。
                并且之后的记录的序号是不连续的。
                若获取每科前两名，只需对排序后的结果增加二次查询即可
                 select * from 
                    (select rank() over(partition by KEMU order by FENSHU desc) as sort_id,student.* from student) st 
                 where st.sort_id<=2;
    
        2)用法2：RANK WITHIN GROUP
            语法： RANK( expr1 ) WITHIN GROUP ( ORDER BY expr2 )
                为聚合函数，返回一个值。
            参数：expr1为1个或多个常量表达式；
                         expr2为如下格式的表达式：    
                          expr2的格式为'expr3 [ DESC | ASC ] [ NULLS { FIRST | LAST } ]'
                其中，expr1需要与expr2相匹配，
                    即：expr1的常量表达式的类型、数量必须与ORDER BY子句后的expr2表达式的类型、数量相同
                    实际是expr1需要与expr3相匹配
                    如：RANK(a) WITHIN GROUP (ORDER BY b ASC NULLS FIRST);
                        其中，a为常量，b需要是与相同类型的表达式
                        RANK(a,b) WITHIN GROUP (ORDER BY c DESC NULLS LAST, d DESC NULLS LAST);
                        其中，a与b都为常量；c是与a类型相同的表达式、d是与b类型相同的表达式；
                
            返回：返回数值型数据，该值为假定记录在表中的序号。
            作用：确定一条假定的记录，在表中排序后的序号。
               如：假定一条记录（假设为r1）的expr2指定字段值为常量expr1，则将r1插入表中后，
                与原表中的记录，按照ORDER BY expr2排序后，该记录r1在表中的序号为多少，返回该序号。
            注释： NULLS FIRST指定，将ORDER BY指定的排序字段为空值的记录放在前边；
                NULLS LAST指定，将ORDER BY指定的排序字段为空值的记录放在后边；
            实例：假设一个员工的薪水为1500，求该员工的薪水在员工表中的排名为多少？
                已知员工表如下：
                SQL> select * from employees;
                EMP_ID     EMP_NAME     SALARY
                ---------- -------------------- ---------------
     ZhangSan             500
     LiSi                         1000
     WangWu               1500
     MaLiu                     2000
     NiuQi                      2500

                SQL> select rank(1500) within group (order by salary) as "rank number" from employees;
                rank number
                -----------
                由结果可知，薪水为1500的员工，在表中按升序排序，序号为3
                
7）FIRST、LAST
        语法：    agg_function（e1） KEEP (DENSE_RANK FIRST ORDER BY e2 [NULLS {FIRST|LAST}]) [OVER PARTITION BY e3 ]
            agg_function（e1） KEEP (DENSE_RANK LAST  ORDER BY e2 [NULLS {FIRST|LAST}]) [OVER PARTITION BY e3 ]                  
        参数：    agg_function为一个聚合函数，可以为 MIN、MAX、SUM、AVG、COUNT、VARIANCE或STDDEV
            e2指定以哪个字段为依据，进行排序；
            e3指定以哪个字段为依据，进行分类（分组）；
            当指定OVER PARTITION BY子句后，针对分类后的每个类单独排序；
            DENSE_RANK为排序后的记录分配序号，并且序号为连续的。
            NULLS {FIRST|LAST}指定排序字段e1的值若为空，则拍在序列前边（NULLS FIRST）或者后边（NULLS LAST）
            DENSE_RANK后的FIRST/LAST确定选取通过DENSE_RANK排好序后的序列中，序号最小/最大的记录。序号相同时，返回多条记录
            当序号相同，返回多条记录时，agg_function(e1)聚合函数继续对这多条记录的e1字段做聚合操作。
        作用：    如果agg_function为min(e1)，获取排序后的FIRST或LAST的多条记录中，某字段e1的最小值
            该字段不是排序关键字段e2
        实例：
        已知员工表有薪水字段，奖金字段。要求获取薪水最低的员工中，奖金最高的员工的记录。
        已知表内容如下：
        SQL> select * from employees order by salary;
         EMP_ID     EMP_NAME           SALARY  COMMISSION
        ---------- ---------------------------- ------------  ------------
     ZhangSan                    500        200
     LiSi                                500        300
     WangWu                      500        100
     MaLiu                           2000       500
     NiuQi                            2500       200
     ShangDuo                   2500       300
     BaiQi                             2500       400
        
        SQL> select max(commission) keep(dense_rank first order by salary asc) as commission from employees;
        COMMISSION
        ----------
        首先，按salary排序后，获取薪水最低的记录，分别为员工10001、10002、10003三条记录。
        聚合函数max(commission)对3条记录获取奖金最高的为员工10002，奖金为300。

聚集函数
```

####    4.5、分组查询

```mysql

分组
    select num from 表 group by num
    select num,nid from 表 group by num,nid
    select num,nid from 表  where nid > 10 group by num,nid order nid desc
    select num,nid,count(*),sum(score),max(score),min(score) from 表 group by num,nid
 
    select num from 表 group by num having max(id) > 10
 
    特别的：group by 必须在where之后，order by之前
```

#### 4.6多表查询

```mysql
a、连表
    无对应关系则不显示
    select A.num, A.name, B.name
    from A,B
    Where A.nid = B.nid
 
    无对应关系则不显示
    select A.num, A.name, B.name
    from A inner join B
    on A.nid = B.nid
 
    A表所有显示，如果B中无对应关系，则值为null
    select A.num, A.name, B.name
    from A left join B
    on A.nid = B.nid
 
    B表所有显示，如果B中无对应关系，则值为null
    select A.num, A.name, B.name
    from A right join B
    on A.nid = B.nid
b、组合
    组合，自动处理重合
    select nickname
    from A
    union
    select name
    from B
 
    组合，不处理重合
    select nickname
    from A
    union all
    select name
    from B
```





