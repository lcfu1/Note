# 嵌套子查询

目录：

- [嵌套子查询](#嵌套子查询)
- [相关子查询](#相关子查询)
- [EXISTS和NOT EXISTS](#exists和not-exists)

## 嵌套子查询

在通常的子查询中，子查询是以嵌套的方式写在父查询的WHERE、HAVING、FROM子句中，所以被 称为嵌套子查询。

嵌套子查询的执行过程：

- 子查询首先执行一次。
- 用来自子查询的值确认或取消父查询的候选行。

```
SELECT ename,sal
FROM emp e,(SELECT deptno,AVG(sal) sa
            FROM emp
            GROUP BY deptno) d
WHERE e.deptno=d.deptno
AND e.sal>d.sa
```

## 相关子查询

父查询中的行每被处理一次，子查询就执行一次。

```
SELECT ename,sal
FROM emp e
WHERE sal>(SELECT AVG(sal)
           FROM emp
           WHERE deptno=e.deptno)
```

查询过程：

- 取得父查询的候选行。
- 用候选行被子查询引用列的值执行子查询。
- 用来自子查询的值确认或取消候选行。
- 重复步骤1、2、3，直到父查询中无剩余的候选行。

## EXISTS和NOT EXISTS

- EXISTS判断是否“存在”，具体操作如下：
  - 子查询中如果有记录找到，子查询语句不会继续执行， 返回值为TRUE。
  - 子查询中如果到表的末尾也没有记录找到，返回值为 FALSE。
- EXISTS子查询并没有确切记录返回，只判断是否有记录存在，而且只要找到相关记录，子查询就不需要再执行，然后再进行下面的操作。这样大大提高了语句的执行效率。
- NOT EXISTS正好相反，判断子查询是否没有返回值。如果没有返回值，表达式为真，如果找到一条返回值，则为假。

```
SELECT ename
FROM emp e
WHERE EXISTS (SELECT '1'
             FROM emp
             WHERE mgr=e.empno)
```

注：

- 在EXISTS子句中，并没有确切记录返回，只返回真或假。 所以’1’只是占位用，无实际意义。

```
SELECT ename
FROM emp e
WHERE NOT EXISTS (SELECT '1'
             FROM emp
             WHERE mgr=e.empno)
```

注：

- NOT EXISTS操作符因为运算方法与NOT IN不同，只会返回TRUE或FALSE，不会返回空值，所以不需要考虑子查询去除值的问题。

