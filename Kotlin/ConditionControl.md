## 条件控制

### if语句

- 可以把 if 表达式的结果赋值给一个变量。

- 可以使用 in 运算符来检测某个数字是否在指定区间内，区间格式为 x..y。

如：

```
fun main(args: Array<String>) {
    val a=1
    val b=2
    val c=if(a>=b) a else b
    println(c)      //结果为2
    if(c in 1..3){
        println(c)  //结果为2
    }
}
```

