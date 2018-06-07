# 基本数据类型

目录：

- [数值类型](#数值类型)
- [字符](#字符)
- [字面常量](#字面常量)
- [比较两个数字](#比较两个数字)
- [类型转换](#类型转换)
- [位操作符](#位操作符)
- [布尔](#布尔)
- [数组](#数组)
- [字符串](#字符串)
- [字符串模板](#字符串模板)

## 数值类型

Kotlin的基本**数值类型**：

| 类型   | 占位 | 取值范围                                 |
| ------ | ---- | ---------------------------------------- |
| Byte   | 8    | -128~127                                 |
| Short  | 16   | -32768~32767                             |
| Int    | 32   | -2147483648~2147483647                   |
| Long   | 64   | -9223372036854775808~9223372036854775807 |
| Float  | 32   | 1.4E-45~3.4028235E38                     |
| Double | 64   | 4.9E-324~1.7976931348623157E308          |

**注**：不同于Java的是，字符不属于数值类型，是一个独立的数据类型。 

可用以下方式获得取值范围：

```
fun main(args: Array<String>) {
	println(Byte.MIN_VALUE)
	println(Byte.MAX_VALUE)
   
	println(Short.MIN_VALUE)
	println(Short.MAX_VALUE)
    
	println(Int.MIN_VALUE)
	println(Int.MAX_VALUE)
    
	println(Long.MIN_VALUE)
	println(Long.MAX_VALUE)
    
	println(Float.MIN_VALUE)
	println(Float.MAX_VALUE)
    
	println(Double.MIN_VALUE)
	println(Double.MAX_VALUE)
}
```

## 字符

- Kotlin 中的 Char 不能直接和数字操作。
- Char 必需是单引号 ' 包含起来的。
- 特殊字符可以用反斜杠转义。 
-  转义序列：\t、 \b、\n、\r、\'、\"、\\ 和 \$。 
- 编码其他字符要用 Unicode 转义序列语法：'\uFF00'。 

```
fun a(c: Char): Int {
    if (c !in '0'..'9')
        throw IllegalArgumentException("Out of range")
    return c.toInt()-'0'.toInt()     // 显式转换为数字
}
fun b(c: Char) {
//     if (c == 1) { // 错误：类型不兼容
        
//     }
    if (c == '1') {  //ok
        println(c)
    }
}
fun main(args: Array<String>) {
    println(a('1'))
    b('1')
}
```

## 字面常量

所有类型的字面常量：

- 十进制：123456789
- 长整型以大写的 L 结尾：12L
- 16 进制以 0x 开头：0x0F
- 2 进制以 0b 开头：0b00001011
- 支持传统符号表示的浮点数值
  - Doubles 默认写法: 10.2e10
  - Floats 使用 f 或者 F 后缀：10.2f
- 使用下划线使数字常量更易读：1_000_000

**注**：不支持8进制

## 比较两个数字

三个等号 === 表示比较对象地址，两个 == 表示比较两个值大小 。

```
fun main(args: Array<String>) {
    val a: Int = 128 
    val b: Int? = a
    val c: Int? = a
    println(a === a)  // true，值相等，对象地址相等
    println(b === c)  //  false，值相等，但对象地址不一样
    println(b == c)   // true，值相等
}
```

**注**：在范围是 [-128, 127] 之间并不会创建新的对象，比较输出的都是 true，从 128 开始，比较的结果才为 false。 

## 类型转换

- 较小的类型不能隐式转换为较大的类型。
- 每种数据类型都有`to+类型()`方法，如toInt()。
- 可以使用自动类型转化的，前提是可以根据上下文环境推断出正确的数据类型而且数学操作符会做相应的重载，如`Long + Int => Long`。

```
fun main(args: Array<String>) {
    val a: Byte = 1
    //val b: Int = a
    //println(b)   //出错
    val c: Int = a.toInt()
    println(c)    //结果为1，Int类型
    val d=1L+2    //Long+Int => Long
    println(d)    //结果为3，Long类型
}
```

## 位操作符

对于Int和Long类型，还有一系列的位操作符可以使用，分别是：

```
shl(bits) – 左移位 (Java’s <<)
shr(bits) – 右移位 (Java’s >>)
ushr(bits) – 无符号右移位 (Java’s >>>)
and(bits) – 与
or(bits) – 或
xor(bits) – 异或
inv() – 反向
```

## 布尔

用 Boolean 类型表示，它有两个值：true 和 false。 

若需要可空引用布尔会被装箱。 

布尔运算有：

```
|| – 短路逻辑或
&& – 短路逻辑与
! - 逻辑非
```

## 数组

- 数组用类 Array 实现，并且还有一个 size 属性及 get 和 set 方法，由于使用 [] 重载了 get 和 set 方法，所以我们可以通过下标很方便的获取或者设置数组对应位置的值。
- 数组的创建两种方式：使用函数arrayOf()和使用工厂函数。
- 除了类Array，还有ByteArray, ShortArray, IntArray，用来表示各个类型的数组，省去了装箱操作，因此效率更高，其用法同Array一样。

```
fun main(args: Array<String>) {
    val a = arrayOf(1,2,3)   //[1,2,3]
    val b = Array(4,{i->i})  //[0,1,2,3]
    val c: IntArray = intArrayOf(1, 2, 3)  //[1,2,3]
    println(a[0])  //值为1
    println(b[3])  //值为3
    println(c[2])  //值为3
}
```

**注**：与 Java 不同的是，Kotlin 中数组是不型变的（invariant）。 

## 字符串

- 和 Java 一样，String 是可不变的。方括号 [] 语法可以很方便的获取字符串中的某个字符，也可以通过 for 循环来遍历。
- 三个引号 """ 扩起来的字符串，支持多行字符串。
- String 可以通过 trimMargin() 方法来删除多余的空白。 

```
fun main(args: Array<String>) {
    val text = """
    多行字符串
    多行字符串
    """
    println(text)   // 输出有一些前置空格
    
    val a="abcd"
    for (i in a) {
    	println(i)  //循环输出abcd
	}
    
    val text1 = """
    |多行字符串
	|多行字符串
    """
    println(text1.trimMargin())    // 前置空格删除了
}
```

## 字符串模板

- 字符串可以包含模板表达式。
- 模板表达式以美元符（$）开头，由一个简单的名字构成。
- 表达式也可以用花括号扩起来。

```
fun main(args: Array<String>) {
    val a=1
    val b="a = $a"
    val c="a = ${a}"
    val d="${'$'}10"
    println(b)   //a=1
    println(c)   //a=1
    println(d)   //$10
}
```

