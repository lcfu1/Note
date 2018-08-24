# 多表连接

目录：

- [连接](#连接)
- [笛卡尔积](#笛卡尔积)
- [等值连接](#等值连接)
- [表别名](#表别名)
- [非等值连接](#非等值连接)
- [外部连接](#外部连接)
- [自身连接](#自身连接)
- [SQL:1999语法的连接](#sql:1999语法的连接)
- [交叉连接](#交叉连接)
- [自然连接](#自然连接)
- [USING子句](#using子句)
- [ON子句](#on子句)
- [左外连接](#左外连接)
- [右外连接](#右外连接)
- [全外连接](#全外连接)

## 连接

- 连接是在多个表之间通过一定的连接条件，使表之间发生关联，进而能从多个表之间获取数据。 

- 在WHERE子句中书写连接条件

- 如果在多个表中出现相同的列名，则需要使用表名作为来自该表的列名的前缀。
- N个表相连时，至少需要N－1个连接条件。

## 笛卡尔积

- 第一个表中的所有行和第二个表中的所有行都发生连接。 
- 为了避免笛卡尔积的产生，通常需要在WHERE子句中包含 一个有效的连接条件。
- 产生： 
  - 连接条件被省略。
  - 连接条件是无效的。

```
SELECT emp.ename,dept.deptno
FROM emp,dept
```

注：emp表14行，dept表4行，上面得到的结果有14*4=56行。

## 等值连接

```
SELECT emp.ename,dept.deptno
FROM emp,dept
WHERE emp.deptno=dept.deptno
```

## 表别名

```
SELECT e.ename,d.dname
FROM emp e,dept d
WHERE e.deptno=d.deptno
```

## 非等值连接

```
SELECT e.ename,e.sal,s.grade
FROM emp e,salgrade s
WHERE e.sal BETWEEN s.losal AND s.hisal
```

## 外部连接

在多表连接时，可以使用外部连接来查看哪些行，按照连接条件没有被匹配上。 

外部连接的符号是(+)。

```
SELECT e.ename,d.dname,d.deptno
FROM emp e,dept d
WHERE e.deptno(+)=d.deptno
```

## 自身连接

也叫自连接，是一个表通过某种条件和本身进行连接的一 种方式，就如同多个表连接一样。

```
SELECT e.ename,m.ename 经理
FROM emp e,emp m
WHERE e.mgr=m.empno
```

## SQL:1999语法的连接

ANSI SQL:1999标准的连接语法

- Oracle除了上述自己的连接语法外，同时支持美国国家 标准协会（ANSI）的SQL:1999标准的连接语法。

## 交叉连接

交叉连接会产生两个表的交叉乘积，和两个表之间的笛卡尔积是一样的；

使用CROSS JOIN子句完成。

```
SELECT e.ename,d.dname
FROM emp e
CROSS JOIN dept d
```

## 自然连接

- 自然连接是对两个表之间相同名字和数据类型的列进行的等值连接；
- 如果两个表之间相同名称的列的数据类型不同，则会产生错误；
- 使用NATURAL JOIN子句来完成。

```
SELECT ename,dname
FROM emp
NATURAL JOIN dept
```

## USING子句

自然连接是使用所有名称和数据类型相匹配的列作为连接条件，而USING子句可以指定用某个或某几个相同名字和数据类型的列作为连接条件。

```
SELECT deptno,e.ename,d.dname
FROM emp e 
JOIN dept d USING(deptno)
WHERE deptno=20
```

注： 

- 如果有若干个列名称相同但数据类型不同，自然连接子句可以用USING子句来替换，以指定产生等值连接的列。
- 如果有多于一个列都匹配的情况，使用USING子句只能指定其中的一列。
- USING子句中的用到的列不能使用表名和别名作为前缀。
- NATURAL JOIN子句和USING子句是相互排斥的，不能同时使用。

## ON子句

- 自然连接条件基本上是具有相同列名的表之间的等值连接。
- 如果要指定任意连接条件,或指定要连接的列，则可以使用ON子句。
- 用ON将连接条件和其它检索条件分隔开，其它检索条件写在WHERE子句。
- ON子句可以提高代码的可读性。

```
SELECT e.deptno,e.ename,d.dname
FROM emp e
JOIN dept d
ON e.deptno=d.deptno
WHERE e.deptno=20
```

## 左外连接

左外连接以FROM子句中的左边表为基表，该表所有行数据按照连接条件无论是否与右边表能匹配上，都会被显示出来。

```
SELECT e.deptno,e.ename,d.dname
FROM emp e
LEFT OUTER JOIN dept d
ON e.deptno=d.deptno
```

## 右外连接

右外连接以FROM子句中的右边表为基表，该表所有行数据按照连接条件无论是否与左边表能匹配上，都会被显示出来。

```
SELECT e.deptno,e.ename,d.dname
FROM emp e
RIGHT OUTER JOIN dept d
ON e.deptno=d.deptno
```

## 全外连接

全外连接返回两个表等值连接结果，以及两个表中所有等值连接失败的记录。

```
SELECT e.deptno,e.ename,d.dname
FROM emp e
FULL OUTER JOIN dept d
ON e.deptno=d.deptno
```