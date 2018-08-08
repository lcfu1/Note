# 分组函数

目录：

- [分组函数](#分组函数)
- [GROUP BY子句](#groupby子句)
- [HAVING子句](#having子句)
- [组函数和多表连接](#组函数和多表连接)
- [组函数的嵌套](#组函数的嵌套)

## 分组函数

分组函数是对表中一组记录进行操作，每组只返回一个结果，即首先要对表记录进行分组，然后再进行操作汇总，每组返 回一个结果，分组时可能是整个表分为一组，也可能根据条件分成多组。

分组函数常用到以下五个函数：

- MIN
- MAX
- SUM
- AVG
- COUNT

注：

- MIN和MAX函数主要是返回每组的最小值和最大值。 

  ```
  SELECT hiredate
  FROM emp
  ```

- MIN和MAX可以用于任何数据类型。

- SUM和AVG函数分别返回每组的总和及平均值。 

  ```
  SELECT SUM(sal),AVG(sal)
  FROM emp
  ```

- SUM和AVG函数都是只能够对数值类型的列或表达式操作。 

- COUNT函数返回满足条件的非空(NULL)行的数量 。 

- COUNT(*)：返回表中满足条件的行记录数。

- 组函数中的DISTINCT会消除重复记录后再使用组函数。

  ```
  SELECT COUNT(DISTINCT deptno)
  FROM emp
  ```

- 除了COUNT(*)之外，其它所有分组函数都会忽略列中的空值，然后再进行计算。

- 在分组函数中使用NVL函数可以使分组函数强制包含含有空值的记录。

  ```
  SELECT COUNT(comm)
  FROM emp
  
  SELECT COUNT(NVL(comm,0))
  FROM emp
  
  结果分别为：
  4
  14
  ```

## GROUP BY子句

- GROUP BY子句可将表中满足WHERE条件的记录按照指定的列划分成若干个小组。

  ```
  SELECT deptno,SUM(sal)
  FROM emp
  GROUP BY deptno
  ```

- 在SELECT列表中除了分组函数那些项，所有列都必须包含在GROUP BY子句中。

  ```
  SELECT ename,deptno,SUM(sal)
  FROM emp
  GROUP BY ename,deptno
  ```

- GROUP BY所指定的列并不是必须出现在SELECT列表中。

  ```
  SELECT SUM(sal)
  FROM emp
  GROUP BY deptno
  ```


## HAVING子句

使用HAVING子句限制组

- 记录已经分组。
- 使用过组函数。
- 与HAVING子句匹配的结果才输出。

```
SELECT deptno,AVG(sal)
FROM emp
GROUP BY deptno
HAVING AVG(sal)>2000
```

## 组函数和多表连接

```
SELECT e.deptno,d.dname,COUNT(*)
FROM emp e,dept d
WHERE e.deptno=d.deptno
GROUP BY e.deptno,d.dname
```

## 组函数的嵌套

与单行函数不同，组函数只能嵌套两层。

```
SELECT deptno,COUNT(NVL(comm,0))
FROM emp
GROUP BY deptno
```

