## 条件控制

### if表达式

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

### When表达式

- 既可以被当做表达式使用也可以被当做语句使用。 
- 如果它被当做表达式，符合条件的分支的值就是整个表达式的值，如果当做语句使用， 则忽略个别分支的值。 
- 类似其他语言的 switch 操作符，else 同 switch 的 default，如果其他分支都不满足条件将会求值 else 分支。 
- 如果很多分支需要用相同的方式处理，则可以把多个分支条件放在一起，用逗号分隔。
- 也可以检测一个值在（in）或者不在（!in）一个区间或者集合中。
- 检测一个值是（is）或者不是（!is）一个特定类型的值。 
- when 也可以用来取代 if-else if链，如果不提供参数，所有的分支条件都是简单的布尔表达式。

```
fun main(args: Array<String>) {
    var x=6
    var validNumbers=setOf(6, 7)
    when (x) {
    	0 -> print("x == $x")
    	1, 2 -> print("x == 1 or x == 2")
    	in 3..5 -> print("x在区间内")
        in validNumbers -> print("x在集合中")
        else -> {
        	print("都不符合")
    	}
	}
}
```

