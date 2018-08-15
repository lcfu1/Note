# 子查询

目录：

- [单行子查询](#单行子查询)
- [多行子查询](#多行子查询)
- [多列子查询](#多列子查询)
- [在FROM子句中使用子查询](#在from子句中使用子查询)
- [ROWNUM](#rownum)
- [TOP-N查询](#top-n查询)

括号内的查询叫做子查询，也叫内部查询，先于主查询执行 。

子查询的结果被主查询（外部查询）使用。

expr operator包括比较运算符：

- 单行运算符：>、<、=、>=、<>、<=。
- 多行运算符：IN、ANY、ALL。

子查询可以嵌于以下SQL子句中：

- FROM子句
- WHERE子句
- HAVING子句

使用：

- 子查询要用括号括起来。
- 将子查询放在比较运算符的右边。
- 对于单行子查询要使用单行运算符。
- 对于多行子查询要使用多行运算符

## 单行子查询

子查询只返回一行一列。

使用单行运算符。

```
SELECT ename,sal
FROM emp
WHERE sal=
      (SELECT MAX(sal)
      FROM emp)
```

## 多行子查询

子查询返回记录的条数可以是一条或多条。

和多行子查询进行比较时，需要使用多行操作符，多行操作符包括：IN、ANY和ALL。

### IN

```
SELECT deptno,ename,sal
FROM emp
WHERE sal IN
      (SELECT sal
      FROM emp
      WHERE deptno=20)
```

### ANY

表示和子查询的任意一行结果进行比较，有一个满足条件即可。

- `< ANY`：表示小于子查询结果集中的任意一个，即小于最大值就可以。
- `>ANY`：表示大于子查询结果集中的任意一个，即大于最小值就可以。
- `= ANY`：表示等于子查询结果中的任意一个，即等于谁都可以，相当于IN。

```
SELECT deptno,ename,sal
FROM emp
WHERE sal > ANY
      (SELECT sal
      FROM emp
      WHERE deptno=20)
AND deptno <> 10
```

### ALL

表示和子查询的所有行结果进行比较，每一行必须都满足条件。

- `< ALL`：表示小于子查询结果集中的所有行，即小于最小值。
- `> ALL`：表示大于子查询结果集中的所有行，即大于最大值。
- `= ALL`：表示等于子查询结果集中的所有行，即等于所有值，通常无意义。

```
SELECT deptno,ename,sal
FROM emp
WHERE sal > ALL
      (SELECT sal
      FROM emp
      WHERE deptno=30)
AND deptno <> 10
```

## 多列子查询

- 多列子查询可以在一个条件表达式内同时和子查询的多个列进行比较。
- 多列子查询通常用IN操作符完成。
- 如果子查询的结果中有一条空值，这条空值导致主查询没有记录返回。 

```
SELECT ename
FROM emp
WHERE empno NOT IN
                (SELECT nvl(mgr,0)
                FROM emp)
```

## 在FROM子句中使用子查询

```
SELECT a.ename,a.deptno,b.sa
FROM emp a,(SELECT deptno,AVG(sal) sa
            FROM emp
            GROUP BY deptno) b
WHERE a.deptno=b.deptno
```

## ROWNUM

- ROWNUM是一个伪列，伪列是使用上类似于表中的列，而实际并没有存储在表中的特殊列。

- ROWNUM的功能是在每次查询时，返回结果集的顺序号，这个顺序号是在记录输出时才一步一步产生的，第一行 显示为1，第二行为2，以此类推。

```
SELECT ROWNUM,ename
FROM emp
```

## TOP-N查询

Top-N查询主要是实现表中按照某个列排序，输出最大或最小的N条记录功能。 

- ASC：查询最小的N条记录。
- DESC：查询最大的N条记录。