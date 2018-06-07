# 循环控制

目录：

- [for循环](#for循环)
- [while循环](#while循环)
- [do...while循环](#do...while循环)
- [返回和跳转](#返回和跳转)

## for循环

```
fun main(args: Array<String>) {
    val items=listOf("a","b","c")
    for (item in items){
        println(item)
    }
    //通过索引遍历list
    for (index in items.indices){
        println(items[index])
    }
}
```

- for 循环可以对任何提供迭代器（iterator）的对象进行遍历。
- 循环体可以是一个代码块。
- 可以通过索引遍历一个数组或者一个 list。

## while循环

```
fun main(args: Array<String>) {
    var a=3
    while (a>0){
        println(a) //结果3、2、1
        a--
    }
}
```

- 满足条件就进入循环。

## do...while循环

```
fun main(args: Array<String>) {
    var a=0
    do{
        println(a) //结果为0
        a--
    }while(a>0)
}
```

- 至少会执行一次。

## 返回和跳转

Kotlin 有三种结构化跳转表达式：

- return：默认从最直接包围它的函数或者匿名函数返回。
- break：终止最直接包围它的循环。
- continue：继续下一次最直接包围它的循环。

```
fun main(args: Array<String>) {
    for (i in 1..10) {
        if (i==3){
            continue   //i为3时,跳过当前循环，继续下一次循环
        }
        println(i)
        if (i>5) break   //i为6时,跳出循环
    }
}
```
