# 类和对象

目录：

- [类定义](#类定义)
- [类属性](#类属性)
- [主构造器](#主构造器)
- [次构造函数](#次构造函数)
- [抽象类](#抽象类)

## 类定义

- 构造函数和初始化代码块、函数、属性、内部类、对象声明。
-  使用 class 关键字来声明类。
- 可定义一个空类。
- 可在类中定义成员函数。

```
class 类名{
    类体的内容
}

//空类
class Empty

//在类中定义成员函数
class a{
    fun b(){
        println("成员函数");
    }
}
```
## 类属性

- 使用关键字var声明为可变的。
- 使用关键字val声明为不可变的。
- 在kotlin中没有new关键字，我们可以像使用普通函数那样使用构造函数创建类实例。
- 属性的使用，使用 . 号来引用。
- 类可以有一个主构造器，以及一个或多个次构造器，主构造器是类头部的一部分，位于类名称之后。
- 如果主构造器没有任何注解，也没有任何可见度修饰符，那么constructor关键字可以省略。
- val不允许设置setter函数，因为它是只读的。
- 类不能有字段。提供了 Backing Fields(后端变量) 机制，备用字段使用field关键字声明，field 关键词只能用于属性的访问器。
- 非空属性必须在定义的时候初始化，kotlin提供了一种可以延迟初始化的方案，即使用 lateinit 关键字来描述属性。

```
class f{
    var a:Int=1
    val b:Int=2
}

//kotlin中没有new关键字
val c = f()

//使用 . 号来引用属性
c.a

class f constructor(a: Int) {}

var allByDefault: Int? // 错误: 需要一个初始化语句, 默认实现了 getter 和 setter 方法
var initialized = 1    // 类型为 Int, 默认实现了 getter 和 setter
val simple: Int?       // 类型为 Int ，默认实现 getter ，但必须在构造函数中初始化
val inferredType = 1   // 类型为 Int 类型,默认实现 getter

//使用 lateinit 关键字来描述属性
public class MyTest {
    lateinit var subject: TestSubject

    @SetUp fun setup() {
        subject = TestSubject()
    }

    @Test fun test() {
        subject.method()  // dereference directly
    }
}
```

例子：

```
class F{
    var a: String = ""
        get() = field   //将变量赋值后转换为大写
        set(value) {
            field = value.toUpperCase()
        }
    var b:Int=2
    	get()=field
    	set
}
fun main(args: Array<String>) {
    var f:F = F()
    f.a = "abc"
    
    println(f.a)
    println(f.b)
}

结果：
ABC
2
```

## 主构造器

- 主构造器中不能包含任何代码，初始化代码可以放在初始化代码段中，初始化代码段使用 init 关键字作为前缀。 
- 主构造器的参数可以在初始化代码段中使用，也可以在类主体定义的属性初始化代码中使用。 
- 如果构造器有注解，或者有可见度修饰符，这时constructor关键字是必须的，注解和修饰符要放在它之前。 

```
class F constructor(a: Int) {
    init {
        println(a)
    }
    fun B() {
        println("类的函数")
    }
}

fun main(args: Array<String>) {
    val f=F(2)
    f.B()
}

结果：
2
类的函数
```

## 次构造函数

- 类可以有二级构造函数，需要加前缀 constructor。
- 如果类有主构造函数，每个次构造函数都要，或直接或间接通过另一个次构造函数代理主构造函数。在同一个类中代理另一个构造函数使用 this 关键字。
- 如果一个非抽象类没有声明构造函数(主构造函数或次构造函数)，它会产生一个没有参数的构造函数。构造函数是 public 。如果你不想你的类有公共的构造函数，你就得声明一个空的主构造函数。

```
//使用 this 关键字代理另一个构造函数
class A(val name: String) {
    constructor (name: String, age:Int) : this(name) {
    }
}

//声明一个空的主构造函数
class B private constructor () {
}
```

例子：

```
class F constructor(a: Int) {
    init {
        println(a)
    }
    // 次构造函数
    constructor (a: Int, b: Int) : this(a) {
        println(a+b)
    }
    fun B() {
        println("类的函数")
    }
}

fun main(args: Array<String>) {
    val f=F(2,3)
    f.B()
}

结果：
2
5
类的函数
```

## 抽象类

- 类本身，或类中的部分成员，都可以声明为abstract的。
- 抽象成员在类中不存在具体的实现。 
- 无需对抽象类或抽象成员标注open注解。 

```
open class Base {
    open fun f() {}
}

abstract class Derived : Base() {
    override abstract fun f()
}
```

