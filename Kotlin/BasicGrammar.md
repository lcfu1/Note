## 基础语法

### 变量

- 常量与变量都可以没有初始化值,但是在引用前必须初始化。
- 声明时可以不指定类型，由编译器判断。 

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

- 函数定义使用关键字 fun，参数格式为：参数 : 类型。
- 表达式作为函数体，返回类型自动推断。

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

