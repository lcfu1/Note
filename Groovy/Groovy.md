# Groovy入门

目录：

- [简介](#简介)
- [安装和使用](#安装和使用)
- [简单例子 ](#简单例子)
- [包](#包)
- [基本数据类型](#基本数据类型)
- [assert方法和Groovy真值](#assert方法和Groovy真值)
- [操作符重载](#操作符重载)
- [集合](#集合)
- [POGOs](#POGOs)
- [Gradle构建文件中的Groovy](#Gradle构建文件中的Groovy)

## 简介

Groovy是一个通用的、面向对象的、基于JVM（Java虚拟机）的编程语言，编译成Java二进制代码。

**官网**：

[http://www.groovy-lang.org/](http://www.groovy-lang.org/)

**下载**：

[http://www.groovy-lang.org/download.html](http://www.groovy-lang.org/download.html)

**w3cschool的Groovy教程**：

[https://www.w3cschool.cn/groovy/groovy_environment.html](https://www.w3cschool.cn/groovy/groovy_environment.html)

## 安装和使用

**安装：**

- 如果电脑上没有 jdk，要先安装 jdk 。
- 去官网下载Groovy然后进行安装。
- 安装过程中有选项的全部勾选上。

**使用：**

- 打开Windows命令提示符（cmd.exe，快捷键windows+r），使用`groovysh`命令进入groovysh。
- 也可以进入Groovy安装目录->bin目录下，点击groovysh应用程序来打开groovysh。
- 打开groovysh后就可以使用groovy命令了。
- 不想使用命令的方式也可以进入Groovy安装目录->bin目录下，点击groovyConsole应用程序来打开groovy控制台，在这里码。

## 简单例子

```
String a='Groovy'
println 'hello'
println 'hello';
println ('hello')
println 'hello,${a}'
println "hello,${a}"

结果：
hello
hello
hello
hello,${a}
hello,Groovy
```

**注：**

- 分号是可选的，可不加。
- 括号也是可选的，也可以不加。
- Groovy中有两种类型的字符串：
  - 单引号字符串：是java字符串，java.lang.String的实例。
  - 双引号字符串：是Groovy字符串，允许插值篡改。

## 包

Groovy中会自动导入以下包：

- java.lang
- java.util
- java.io
- java.net
- groovy.lang
- groovy.util

## 基本数据类型

Groovy中没有原始类型，所有的变量都使用包装类，如java.lang.Integer、java.lang.Character、java.lang.Double等。

内置的数据类型整数的文法，是Integer类型，如1、2、3等。

内置的数据类型浮点数的文法，是java.math.BigDecimal类型，如1.2等。

```
String a='Groovy'
println "hello,${a}"
println "hello,$a"
Date b=new Date()
println b
def c=1
c='hi'
println c

结果：
hello,Groovy
hello,Groovy
Sat May 26 00:05:33 CST 2018
hi
```

**注：**

- Groovy允许使用一个实际类型，如String、Date、Employee定义变量等，也可以使用def关键字。使用def关键字的是动态数据类型，可改变，而使用String等直接声明的是静态数据类型的。

- "hello,${a}"是字符串插值，完整形式。
- "hello,$a"也是符串插值，当没有歧义时，可以使用类似此种的简短形式。

## assert方法和Groovy真值

Groovy的**assert方法**是根据**Groovy真值**来评估参数的：

- 非零数字，如正、负数，是真。
- 非空集合，如字符串，是真。
- 非null的引用，是真。
- Boolean类型的true，是真。

**例子**：

```
assert 1                 //正数，是真
//assert 0                 //0，会抛出异常
assert -1                //负数，是真
assert 'a'               //字符串，是真
assert !0                //是真
assert !''               //是真
assert !""               //是真
assert ![]               //是真
assert 1==1              //Boolean类型，且为true，是真
assert 10==2+1+3+4+1     //Boolean类型，为false，会抛出异常
```

通过的断言不返回任何东西，失败的断言会抛出一个异常，还包含大量的调试信息。

如上面的`assert 10==2+1+3+4+1`，是Boolean类型，但为false，所以会抛出异常，且会输出以下详细的调试信息：

```
Exception thrown

Assertion failed: 

assert 10==2+1+3+4+1     //Boolean类型，为false，会抛出异常
         |  | | | |
         |  3 6 | 11
         false  10


	at ConsoleScript21.run(ConsoleScript21:10)
```

## 操作符重载

在Groovy中，每个操作符都对应一个方法的调用，如+加号调用Number的plus方法、*乘号调用Number的multiply方法、**则表示求幂操作符、==调用的是equals方法，检查的是相等性、is方法用来检查引用。

```
assert 1+2==1.plus(2)
assert 2-1==2.minus(1)
assert 1*2==1.multiply(2)
assert 2/1==2.div(1)
assert 'Groovy'*2=='GroovyGroovy'
assert 2**4==16
Integer a=1
Integer b=a
assert a.is(b)

结果：
都为真，无输出
```

## 集合

使用方括号，并用逗号分隔，可以创建一个ArrayList，可以使用as操作符来转换一个集合的类型，此外，集合也有操作符重载，实现方法如plus、minus等。

```
def a=[1,2,3]
assert a instanceof ArrayList
Set b=a as Set
assert b==[1,2,3] as Set
assert b instanceof Set

assert a[0..2]==[1,2,3]
assert a[-1..-3]==[3,2,1]                    //从末尾向前调用
assert a[-1..1]==[3,2]					   //也是从末尾向前调用

String c='Groovy'
assert 'yvoorG'==c[-1..0]                    //字符串也是集合

def map=[a:1,b:2]
assert map.getClass()==LinkedHashMap
assert map.a==1                               //重载的点号是put方法
assert map['a']==1                            //使用putAt方法
assert map.get('a')==1                       //java方式

结果：
都为真，无输出
```

**注：**

- instanceof运算符：该运算符是二目运算符，左边的操作元是一个对象，右边是一个类，当左边的对象是右边的类或子类创建的对象时，该运算符运算的结果是true，否则为false。
- Range包含两个值，用两个点号隔开，如a[0..2]。
- Maps使用冒号分隔键和值，Map上的方括号就是getAt或putAt方法。

## 闭包

闭包就像java中的lambda，接受一个参数并解析一块代码，Groovy闭包可以修改在其之外定义的变量。

Groovy中有一个Closure类，代表一块代码。

Groovy中的很多方法都使用闭包作为参数，如集合each方法，它的每个元素都是一个闭包，不过是一同被解析的。

在闭包中修改一个闭包外定义的变量不是一个好方法。

```
def a=[1,2,3]
def b=[]                  //空数例
a.each{ n->               //each接受一个单参数的闭包
    b << n*2              //左移操作符在集合中追加元素
}
assert b==[2,4,6]
```

以上例子是一个列表值乘二的操作，不过更推荐用以下方法来实现，我们可以用闭包collect方法来将一个集合转换到一个新集合中。

```
--------------------例子(1)-----------------------
def a=[1,2,3]
def b=a.collect{ it*2 }
assert b==[2,4,6]

--------------------例子(2)------------------------
def a=[1,2,3]
def b=a.collect{ n->
    n*2
}
assert b==[2,4,6]
```

**注：**

- 当一个闭包有一个参数的情况，使用箭头操作符时不需要给定参数名，it 为默认的虚拟变量名，如 (1) 。
- 如果想要给定参数名，则参考例子(2)。

## POGOs

Java中只包含属性和getters,setters通常被称为POJOs（Plain Old Java Objects）,而在Groovy中也有类似的类，被称为POGOs（Plain Old Groovy Objects）。

```
import groovy.transform.Canonical
@Canonical
class Event{
    String a
}

Event e=new Event(a:'hello')
Event e1=new Event(a:'hello')
println e						//结果为Event(hello)
println e.toString()			 //结果为Event(hello)
Set s=[e,e1,1,2]                  //因为e和e1相等，所以在s中e和e1被当作只有一个。
println s.size()				 //结果为3
```

**注：**

- Event类默认是public的，方法也默认是public的。
- 属性默认是private的，但属性生成的Getters和Setters是没有默认标记的。
- 一个默认的构造器和一个“基于map”的构造器（以“attribute:value”形式的参数）被提供。
- @Canonical注解触发一个抽象语法树（Abstract Syntax Tree，AST）的转换，而AST转换按照特定的方式修改编译时生成的语法树。
- @Canonical注解是由@ToString、@EqualsAndHashCode、@TupleConstructor三种AST转换的简写。
- @Canonical注解的作用：
  - 一个toString的重载方法显示完整的类名，然后跟随每个属性的值，从上到下。
  - 一个equals重载方法做null的安全检查并检查每个属性的值等性，如上面的e和e1相等。
  - 一个hashCode重载方法基于属性的值生成一个整数。
  - 一个附加的构造函数，依次接受所有属性为参数。

## Gradle构建文件中的Groovy

Gradle构建文件支持所有的Groovy语法。

```
apply plugin: 'com.android.application'

android {
    compileSdkVersion 27
}

task clean(type:Delete){
    delete rootProject.buildDir
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    //implementation fileTree(dir: 'libs', include: ['*.jar'])
}
```

**注：**

- apply是Project实例上的一个方法，方法上的大括号是可选的，使用plugin参数来设置Project实例上的plugin属性。
- android关键字是DSL的一部分，其接受一个闭包参数，闭包内部有属性和方法，如compileSdkVersion就是方法的调用，这里省略了括号。
- 还有嵌套的属性，如compileSdkVersion，可以使用一个点号作为替代，如android.compileSdkVersion=23。
- clean任务是一个Delete类的实例（Tash的子类），并接受一个闭包作为参数。
- 如果一个方法接受闭包为最后一个参数，则这个闭包通常在括号之后被添加。
- rootProject.buildDir调用了一个delete方法，这里省略了括号，rootProject属性的值是顶层项目，buildDir的默认值是“build”，所以delete rootProject.buildDir在这里的意思是删除顶层项目的“build”目录。需要注意的是：在顶层调用clean任务也会调用app子项目，所以delete rootProject.buildDir也会删除app子项目下的“build”目录。
- compile关键字也是DSL的一部分，暗示其参数会在编译阶段被应用。
- fileTree方法使用的括号可以省略。
- dir参数接受一个字符串表示一个本地路径。
- include参数接受一个Groovy的数列的文件名模式。
- 现在使用implementatio来代替compile。