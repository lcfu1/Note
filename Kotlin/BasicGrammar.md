## 基础语法

### 默认导入包

有多个包会默认导入到每个 Kotlin 文件中：

- kotlin.*
- kotlin.annotation.*
- kotlin.collections.*
- kotlin.comparisons.*
- kotlin.io.*
- kotlin.ranges.*
- kotlin.sequences.*
- kotlin.text.*

### 变量

常量与变量都可以没有初始化值,但是在引用前必须初始化。

声明时可以不指定类型，由编译器判断。 

可变变量定义：var 关键字，可多次赋值，如

```
var <标识符> : <类型> = <初始化值>
```

不可变变量定义：val 关键字，只能赋值一次的变量(类似Java中final修饰的变量) ，如：

```
val <标识符> : <类型> = <初始化值>
```

例子：

```
fun main(args:Array<String>){
	val a:Int=1		//指定为Int类型
	val b = 1		//系统自动推断变量类型为Int
	val c: Int      // 如果不在声明时初始化则必须提供变量类型
	c = 1           // 明确赋值
	var d=2
	d+=1			//变量d修改为3

	println(a)
	println(b)
	println(c)
	println(d)
}
```

### 注释

支持单行和多行注释，如：

```
// 单行注释

/* 多行
   注释 */
```

### 函数定义

函数定义使用关键字 fun，参数格式为：参数 : 类型。

表达式作为函数体，返回类型自动推断。

```
fun sum(a: Int, b: Int): Int {   //返回值为Int类型
    return a + b
}

fun sum(a: Int, b: Int) = a + b   //表达式作为函数体,类型自动推断

public fun sum(a: Int, b: Int): Int = a + b   // public 方法则必须明确写出返回类型

fun sum(a: Int, b: Int): Unit { 
    println(a + b)
}

// 如果是返回 Unit类型，则可以省略(对于public方法也是这样)：
public fun sum(a: Int, b: Int) { 
    println(a + b)
}
```

**可变长参数函数**

函数的变长参数可以用 vararg 关键字进行标识。

```
fun vars(vararg v:Int){
	for(i in v){
		println(i)
	}
}
```

**lambda(匿名函数)**

```
val sumLambda: (Int, Int) -> Int = {x,y -> x+y}
fun main(args:Array<String>){
	println(sumLambda(1,2))
}
```

### 字符串模板

$表示一个变量名或者变量值。

$varName 表示变量值。

${varName.fun()} 表示变量的方法返回值。

```
var a = 1
val b = "$a"                       //获得a的变量值
val c = "${b.replace("1", "3")}"   //获得b变量的方法返回值，如b=1,就返回3

fun main(args: Array<String>) {
	println(b)
    println(c)
}
```

### NULL检查机制

Kotlin的空安全设计对于声明可为空的参数， 在使用时要进行空判断处理，有两种处理方式：

字段后加!!像Java一样抛出空异常。

字段后加?可不做处理返回值为null或配合?:做空判断处理。

```
var a: String? = "1"             //类型后面加?表示可为空
val b = a!!.toInt()              //抛出空指针异常
val c = a?.toInt()               //不做处理返回 null
val d = a?.toInt() ?: -1   		 //a为空返回-1
```

### 区间

区间表达式由具有操作符形式 .. 的 rangeTo 函数辅以 in 和 !in 形成。

区间是为任何可比较类型定义的，但对于整型原生类型，它有一个优化的实现。

```
fun main(args: Array<String>) {
    for (i in 1..4) print(i)        // 输出“1234”
    println("\n")
	for (i in 1..8 step 2) print(i)        // 使用 step 指定步长,输出“1357”
    println("\n")
	for (i in 8 downTo 1 step 2) print(i)    // 输出“8642”
    println("\n")
	for (i in 1 until 4) print(i)   //使用 until 函数排除结束元素 i in [1, 4) 排除了 4
}
```

### 类型检测及自动类型转换

使用 is 运算符检测一个表达式是否某类型的一个实例(类似于Java中的instanceof关键字) 。

```
fun getStringLength(obj: Any): Int? {
  if (obj is String) {
    // 做过类型判断以后，obj会被系统自动转换为String类型
    return obj.length 
  }
  return null
}
fun main(args: Array<String>) {
	println(getStringLength("abc")) //输出为3
    println(getStringLength(1))     //输出为null
}
```