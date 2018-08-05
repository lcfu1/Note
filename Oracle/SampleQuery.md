# 简单的查询语句

目录：

- [SQL](#sql)
- [结构化查询语言分类](#结构化查询语言分类)
- [SQL概念和规则](#sql概念和规则)
- [SQL所涉及的运算符和函数](#sql所涉及的运算符和函数)
- [运算符的优先级](#运算符的优先级)
- [基本SELECT语句](#基本select语句)
- [简单的选择与投影查询](#简单的选择与投影查询)
- [查询所有列](#查询所有列)
- [查询指定列](#查询指定列)
- [使用算术运算符](#使用算术运算符)
- [空值NULL](#空值null)
- [列别名](#列别名)
- [连接操作符](#连接操作符)
- [原义字符串](#原义字符串)
- [消除重复行](#消除重复行)
- [显示表的结构](#显示表的结构)
- [比较数值型数据](#比较数值型数据)
- [比较字符型数据](#比较字符型数据)
- [比较日期型数据](#比较日期型数据)
- [BETWEEN AND](#betweenand)
- [IN](#in)
- [LIKE](#like)
- [IS NULL](#isnull)
- [AND](#and)
- [OR](#or)
- [NOT](#not)
- [ORDER BY子句](#orderby子句)
- [按列名升序排序](#按列名升序排序)
- [按列名降序排序](#按列名降序排序)
- [按列别名排序](#按列别名排序)
- [多列参与排序](#多列参与排序)
- [按结果集列序号排序](#按结果集列序号排序)

## SQL

SQL(Structured Query Language)，结构化查询语言。

## 结构化查询语言分类

- 数据查询语言（DQL:Data Query Language）：语句主要包括 SELECT，用于从表中检索数据。
- 数据操作语言（DML：Data Manipulation Language）：语句主 要包括INSERT，UPDATE和DELETE，用于添加，修改和删除表中的 行数据。
- 事务处理语言（TPL:Transaction Process Language）：语句 主要包括COMMIT和ROLLBACK，用于提交和回滚。
- 数据控制语言（DCL:Data Control Language）：语句主要包括 GRANT和REVOKE，用于进行授权和收回权限。
- 数据定义语言（DDL:Data Definition Language）：语句主要包 括CREATE、DROP、ALTER，用于定义、销毁、修改数据库对象。

## SQL概念和规则

相关概念

- 关键字（Keyword）：SQL语言保留的字符串，例如，SELECT 和FROM都是关键字。
- 语句（statement）：一条完整的SQL命令，例如，SELECT * FROM dept 是一条语句。
- 子句（clause）：部分的SQL语句，通常是由关键字加上其它 语法元素构成，例如，SELECT * 是一个子句，FROM table也是一个子句。

书写规则

- 不区分大小写，也就是说SELECT，select，Select，执行时 效果是一样的，关键字建议大写，其它语法元素（如列名、表名等）小写。
- 可以单行来书写，也可以书写多行，建议分多行书写，增强代码可读性，通常以子句为单位进行分行。
- 关键字不可以缩写、分开以及跨行书写，如SELECT不可以写成SEL或SELE CT等形式。
- Tab和缩进的使用可以提高程序的可读性。

## SQL所涉及的运算符和函数

| 名称     | 运算符                                                       | 说明                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 比较运算 | =、!=、>、<、>=、<=、<>、!>、!<、                            | 等于、不等于、大于...                                        |
|          | BETWEEN AND、NOT BETWEEN AND                                 | 介于两者之间、介于两者之外                                   |
| 谓       | IN、NOT IN                                                   | 在其中、不在其中                                             |
| 词       | LIKE、NOT LIKE                                               | 匹配、不匹配                                                 |
|          | IS NULL、IS NOT NULL                                         | 是空值、不是空值                                             |
| 逻辑运算 | NOT、AND、OR                                                 | 非、与、或                                                   |
| 集函数   | COUNT(*)、COUNT(列名)、SUM(列名)、AVG(列名)、MAX(列名)、MIN(列名) | 统计元组个数、统计列值个数、列值汇总、求列值平均、求列值最大、求列值最小 |

## 运算符的优先级

| 优先级 | 运算分类       | 运算符举例                     |
| ------ | -------------- | ------------------------------ |
| 1      | 算术运算符     | +，-，*，\                     |
| 2      | 连接运算符     | \|\|                           |
| 3      | 比较运算符     | =, <>, <, >, <=, >=            |
| 4      | 特殊比较运算符 | BETWEEN AND，IN，LIKE，IS NULL |
| 5      | 逻辑非         | NOT                            |
| 6      | 逻辑与         | AND                            |
| 7      | 逻辑或         | OR                             |

注：

- 括号()优先于其他操作符。

## 基本SELECT语句

File->New->SQL Window，新建一个SQL窗口，在这里写语句。

```
SELECT [DISTINCT]{*|column|expression [alias],...}
FROM table
[WHERE condition(s)]
```

注：

- SELECT子句：表示所需检索的数据列。
- FROM子句：表示检索的数据来自哪个表。
- condition(s)：表示条件表达式。

## 简单的选择与投影查询

- 无条件查询
  - 只有SELECT和FROM组成的简单查询。
- 条件查询
  - 就是使用WHERE子句为查询增加条件，WHERE子句的条件表达式是一个逻辑表达式，由常量、变量、函数、属性名和各种运算符组成。
  - WHERE子句可以返回限定的数据行。

## 查询所有列

```
SELECT *
FROM emp
```

注：

- *：等价于所有的列。

## 查询指定列

```
SELECT ename
FROM emp
```

注：

- ename：指的就是ENAME列。

## 使用算术运算符

| 运算符 | 描述 |
| ------ | ---- |
| +      | 加   |
| -      | 减   |
| *      | 乘   |
| /      | 除   |

**优先级**

- 乘除优先于加减。
- 相同优先权的表达式按照从左至右的顺序依次计算。
- 括号可以提高优先权。

```
SELECT sal,sal+1000
FROM emp
```

注：

- sal+1000：查询的结果为sal+1000的值。

## 空值NULL

- 空值是指一种无效的、未赋值、未知的或不可用的值。
- 空值不同于零或者空格。
- 任何包含空值的算术表达式运算后的结果都为空值NULL。

```
SELECT comm,comm+1000
FROM emp
```

注：

- comm+1000：只有comm不为空值，相加才有结果，不然就为空值。

- 如果comm为空值，但想相加的值不为空值，可以使用nvl函数，如：

  ```
  SELECT comm,nvl(comm+1000,0)
  FROM emp
  ```

## 列别名

用来重新命名列的显示标题。

**使用方法**

- 列名 列别名。
- 列名 AS 列别名。

**以下情况要添加双引号**

- 列别名中包含有空格。
- 列别名中要求区分大小写。
- 列别名中包含有特殊字符。

```
SELECT ename 员工名,job AS 工作,sal "薪水"
FROM emp
```

## 连接操作符

- 用于连接列与列、列和字符。
- 形式上是以两个竖杠||。
- 用于创建字符表达式的结果列。

```
SELECT ename || job
FROM emp
```

## 原义字符串

- 原义字符串是包含在SELECT列表中的一个字符、一个数 字或一个日期。
- 日期和字符字面值必须用单引号引起来。
- 每个原义字符串都会在每个数据行输出中出现。

```
SELECT ename || '的工作是' || job AS "Employee Details"
FROM emp
```

## 消除重复行

```
SELECT DISTINCT job
FROM emp
```

注：

- 使用DISTINCT关键字。

## 显示表的结构

File->New->Command Window，新建一个Command窗口，可以在这个窗口显示表的结构，如：

```
Connected to Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 
Connected as scott@ORCL

SQL> DESC emp
Name     Type         Nullable Default Comments 
-------- ------------ -------- ------- -------- 
EMPNO    NUMBER(4)                              
ENAME    VARCHAR2(10) Y                         
JOB      VARCHAR2(9)  Y                         
MGR      NUMBER(4)    Y                         
HIREDATE DATE         Y                         
SAL      NUMBER(7,2)  Y                         
COMM     NUMBER(7,2)  Y                         
DEPTNO   NUMBER(2)    Y                         

SQL> 
```

注：

- 使用DESC emp命令来查看表结构。



## 比较数值型数据

```
SELECT deptno
FROM emp
WHERE deptno=30
```

## 比较字符型数据

```
SELECT job
FROM emp
WHERE job='SALESMAN'
```

注：

- 字符型数据作为被比较的值时，必须用单引号引起来。
- 字符型数值区分大小写。

## 比较日期型数据

```
SELECT hiredate
FROM emp
WHERE hiredate>'01-1月-81'
```

注：

- 日期型数值作为被比较的值时，必须用单引号引起来。
- 日期型数值是区分日期表达形式的，默认的日期形式是DD-MON-RR。

## BETWEEN AND

用来判断要比较的值是否在某个范围内。

```
SELECT deptno
FROM emp
WHERE deptno BETWEEN 20 AND 30
```

## IN

用来判断要比较的值是否和集合列表中的任何一个值相等。

```
SELECT deptno
FROM emp
WHERE deptno IN(20,30)
```

## LIKE

- 使用LIKE运算符判断要比较的值是否满足部分匹配，也叫模糊查询。
- 模糊查询中两个通配符：
  - %：代表零或任意更多的字符。
  - _：代表一个字符。
  - 如果要查询的字符串本身就含有%或_，可以使用ESCAPT进行转义。
  - ESCAPE '\'短语表示\为换码字符，紧跟在\后面的字符“%”不再具有通配符的含义，而转义为普通的“%”字符。

```
SELECT ename
FROM emp
WHERE ename LIKE 'M%'

SELECT ename
FROM emp
WHERE ename LIKE '_M%'

SELECT ename
FROM emp
WHERE ename LIKE 'S\%%' ESCAPE '\'
```

## IS NULL

使用IS NULL运算符来判断要比较的值是否为空值NULL。

```
SELECT comm
FROM emp
WHERE comm IS NULL
```

## AND

要求两个条件都为真，结果才为真。

```
SELECT ename,job
FROM emp
WHERE ename LIKE 'S%' AND job='CLERK'
```

## OR

只需要两个条件中的一个为真，结果就返回真。

```
SELECT ename,job
FROM emp
WHERE ename LIKE 'S%' OR job='CLERK'
```

## NOT

可以和IN、BETWEEN AND、LIKE、IS NULL一起使用。

```
SELECT job
FROM emp
WHERE job NOT IN('CLERK')
```

## ORDER BY子句

ORDER BY子句能对查询结果集进行排序，语法结构如下：

```
SELECT  [DISTINCT] { * | 列名|表达式[别名][,...]}
FROM表名 [WHERE  条件]
[ORDER BY  {列名|表达式|列别名|列序号} [ASC|DESC],…]
```

注：

- 可以按照列名、表达式、列别名、结果集的列序号排序。
- ASC：升序，默认值。
- DESC：降序。
- ORDER BY 子句必须写在SELECT语句的后。

**排序规则**

- 数字升序排列小值在前，大值在后。即按照数字大小顺序由小到大排列。
- 日期升序排列相对较早的日期在前，较晚的日期在后。
- 字符升序排列按照字母由小到大的顺序排列，即由A-Z排列；中文升序按照字典顺序排列。
- 空值在升序排列中排在后，在降序排列中排在开始。

## 按列名升序排序

```
SELECT sal
FROM emp
ORDER BY sal
```

## 按列名降序排序

```
SELECT sal
FROM emp
ORDER BY sal DESC
```

## 按列别名排序

```
SELECT sal salary
FROM emp
ORDER BY salary
```

## 多列参与排序

```
SELECT sal,comm
FROM emp
ORDER BY sal,comm DESC
```

注：

- 参与排序的多列都可以指定升序或者降序。
- ORDERBY子句中可以写没在SELECT列表中出现的列。

## 按结果集列序号排序

```
SELECT sal,comm
FROM emp
ORDER BY 2
```

注：

- 列名可以用SELECT语句后列的顺序号来代替。

