## SQL语言的分类

SQL语言共分为四大类：

- 数据查询语言DQL

  基本结构是由SELECT子句，FROM子句，WHERE子句组成的查询块：

  ```sql
  SELECT <字段名表>
  FROM <表或视图名>
  WHERE <查询条件>
  ```

- 数据操纵语言DML
  插入：INSERT
  更新：UPDATE
  删除：DELETE

- 数据定义语言DDL

  数据定义语言DDL用来创建数据库中的各种对象-----表、视图、索引、同义词、聚簇等如：

  ```sql
  CREATE TABLE/VIEW/INDEX/SYN/CLUSTER
  						| 	| 		| 		|			 |
  				表   视图  索引  同义词		簇
  ```

- 数据控制语言DCL

  用来授予或回收访问数据库的某种特权，并控制数据库操纵事务发生的时间及效果，对数据库实行监视等。如：

  - GRANT：授权。
  - ROLLBACK [WORK] TO [SAVEPOINT]：回退到某一点。
    回滚---ROLLBACK
    回滚命令使数据库状态回到上次最后提交的状态。其格式为：
    SQL>ROLLBACK;
  - COMMIT [WORK]：提交。





## MySQL中的属性

### 创建数据表的语法

```sql
 create table [if not exists] `表名` ( 
   `字段名1` 列类型 [属性] [约束] [注释] ,
   `字段名2` 列类型 [属性] [约束] [注释] , 
   ......
   `字段名n` 列类型 [属性] [约束] [注释]
 )[ 表字符集 ][注释];
```

注意：大家是否发现上面的语句有 `` 这个符号，这叫做反引号，在英文输入状态下点击tab键 上面的键(esc键下面的键)就可以输出反引号。

如果我们的表名或者字段名是MySQL的关键字，例如"table"，"from"，"select"等等那必须要加上反引 号来区分，一般为了让代码更加严谨，都会加上反引号



### 约束

MySQL中的约束了：

- 主键约束(primary key)

  即主关键字，是被挑选出来，作表的行的唯一标识的，主键列的值不会重复。

  每个表中只能有一个主键。

- 唯一约束(unique)

  指定字段的取值不能重复。

- 自增长约束(auto_increment)

  指定字段的取值自动生成，默认从1开始，每增加一条记录，该字段的取值会加1。

- 外键约束(foreign key)

  某一张表中某字段的值依赖于另一张表中某字段的值。

- 非空约束

约束不是越多越好 ;小数据量的表建议不要加约束;不建议给经常变动的数据加约束 。 

如果要删除了添加了约束的表的话，要先删除从表(引用其他表的表)再删除主表(被引用的表)。



## DML

delete和truncate都能删除数据、但不删除表结构。

delete 和truncate的区别

- truncate删除速度更快 
- 使用truncate会重新设置auto_increment计数器



## DQL

### 条件查询

**select**语句中**where**包含的条件

- 带关系运算符和逻辑运算符的条件数据查询; 
- 带between and关键字的条件查询语句;
-  带is null关键字的条件查询语句;
-  带is not null关键字的条件查询语句; 
- 带in关键字的条件查询语句; 
- 带like关键字的条件查询语句;

$$
\begin{array}{|l|l|l|l|}
\hline \text { 算术运算符 } & \text { 描述 } & \text { 逻辑运算 } & \text { 描述 } \\
\hline> & \text { 大于 } & \text { and(\&\&) } & \text { 逻辑与 } \\
\hline< & \text { 小于 } & \text { or }(\mid \mid) & \text { 逻辑或 } \\
\hline= & \text { 等于 } & \text { xor } & \text { 逻辑异或 } \\
\hline !=(<>) & \text { 不等于 } & \operatorname{not}(!) & \text { 逻辑非 } \\
\hline<= & \text { 小于等于 } & & \\
\hline>= & \text { 大于等于 } & & \\
\hline
\end{array}
$$



## 单表查询

### 常用时间日期型函数

- NOW() :返回当前的日期和时间 
- DATE():提取日期 
- DATEDIFF():返回两个日期之间的天数 
- DATE_FORMAT():用不同格式显示日期/时间

查看所有员工的工作年限

```sql
select ename as 员工姓名,datediff(now(),hiredate)/365 as 工作年限 from employee;
```



### 数据分组、过滤、排序

如果我想要知道每个部⻔的员工人数，每个部⻔员工的平均薪资，那应该如何查询呢?

#### group by(数据分组)

语法参考

```sql
select 字段名,计算字段 from 表名 group by 字段
```

统计每个部⻔的人数

```sql
select deptno as 部⻔,count(*) as 员工人数 from employee group by deptno;
```

统计每个部⻔员工的平均薪资

```sql
select deptno as 部⻔,round(avg(sal),2) as 平均薪资 from employee group by deptno;
```

注:round()，可以注明保留几位小数

#### **having** (数据过滤)

之前我们的筛选条件都是用where，但如果要对分组查询的结果进行条件筛选的话，就不能用where这

个关键字了，要用having。

查询薪资大于**2500**的员工姓名和具体薪资

方法1

```sql
select ename as 员工姓名,sal as 薪资 from employee where sal>2500 group by ename;
```

方法2

```sql
select ename as 员工姓名,sal as 薪资 from employee group by ename having sal>2500;
```

这两种语句得到的最终结果是一样的，但实现概念不一样!

- 方法1是查询除薪资大于2500的员工姓名，具体薪资后分组；
- 方法2是分组后筛选薪资大于2500的情 况。

在实际工作中部分特定场景会出现不一样的结果，这里要注意了！

查询平均薪资大于**2500**的部⻔和具体薪资

```sql
select deptno as 部⻔,round(avg(sal),2) as 平均薪资 from employee group by deptno having avg(sal)>2500;
```

总结:

- having子句即可包含聚合函数作用的字段也可包含普通的标量字段。
- having子句必须与group by子句同时使用，不能单独使用。



#### order by(数据排序)

查询结果乱糟糟的，想让它们按照某列进行排序，怎么做?

语法参考:

```sql
select 字段，计算字段
from 表名
where 条件(非必须)
group by 字段
having 条件(非必须)
order by 字段 (asc desc)
```

默认排序按照asc，也就是升序;需要按照降序排序后面加上desc

查询薪资大于**2500**的员工姓名薪资信息，结果按照薪资降序排序，薪资相同的按照姓名升序排序。

```sql
select ename as 员工姓名,sal as 薪资 from employee group by ename having sal>2500 order by sal desc,ename;
```



#### limit (数据top几查询)

查询薪资最高的员工，用max()函数就可以了;查询薪资最高的前三位呢? 

查询考试成绩获得前五名的同学;报考热度top10的专业等等的问题?

语法参考:

```sql
limit m, n
或者
limit n offset m
```

m:startindex 表示起始位置，从0开始，0为第一条数据;m不指定数则起始位置为0，从第一条开始返 回前n条记录。

n:length表示取几个。

查询薪资前五名的员工姓名和职位

```sql
select ename as 姓名,job as 职位 from employee order by sal desc limit 5;
```

当 limit和offset组合使用的时候，limit后面只能有一个参数，表示要取的的数量，offset表示要跳过的数量 。

查询薪资第**6**到第**10**名的员工姓名和职位

```sql
select ename as 姓名,job as 职位 from employee order by sal desc limit 5,5; 或者
select ename as 姓名,job as 职位 from employee order by sal desc limit 5 offset 5;
```





## 多表连接查询

<img src="./img/SQL Join.png" alt="img" style="zoom:70%;" />



### 子查询

上面的连接查询确实能将两张表格同时查询输出。

但是我只有一张表，想用这张表的一部分数据作为条件去查这张表。

这就要用到子查询了。

查询所有姓王的员工的家庭住址和直属主管

```sql
select 姓名,家庭住址,直属主管 from 员工 where 员工编号 in (select 员工编号 from 员工 where 姓名 like "王%");
```

查询出产品资料表中，所有的日用品

```sql
select * from 产品资料 where 类别编号=(select 类别编号 from 产品类别 where 类别名称 ="日用品");
```

单列查询

- = :返回值和查询的列名必须一致
-  < 或者 >:返回值必须是单行单列的值

多列查询

- in:查询结果在返回字段 结果内
-  not in:查询结果不在返 回字段结果内
-  =any :查询结果在返回 字段结果内任意满足 
- =all:查询结果在返回 字段结果内全部满足



## SQL练习

### 添加主键

```sql
create table score(
学号 varchar(255) not null,
课程 varchar(255) not null,
成绩 float not null)
default charset=utf8;

alter table score add constraint pk_score primary key (学号, 课程);
# 为“score”中的“学号, 课程”字段添加一个约束名为pk_score的主键约束
# 也就是把“学号, 课程”字段设为“学生基本信息表”的主键 
```



### Union All和Union

- UNION会进行数据的**排序和去重**，查询效率低。
- UNION ALL没有进行去重和排序，查询效率高。

需要注意的是：**合并的两个或多个表需要保证它们字段的个数相同，否则会报错**。



### case when

```sql
SELECT
    STUDENT_NAME,
    (CASE WHEN score < 60 THEN '不及格'
        WHEN score >= 60 AND score < 80 THEN '及格'
        WHEN score >= 80 THEN '优秀'
        ELSE '异常' END) AS REMARK
FROM
    TABLE
```

**注意：**如果你想判断score是否**null**的情况，WHEN score **=** null THEN '缺席考试'，这是一种**错误**的写法，**正确的写法**应为：

```sql
CASE WHEN score IS NULL THEN '缺席考试' ELSE '正常' END
```

```sql
select 课程号,
sum(case when 成绩 >= 60 then 1 else 0 end ) as 及格人数,
sum(case when 成绩 < 60 then 1 else 0 end ) as 不及格人数
from score
group by 课程号
```

