# Groovy入门

目录：

- [简介](#简介)
- [安装和使用](#安装和使用)
- [基本语法 ](#基本语法)
- [assert方法和Groovy真值](#assert方法和Groovy真值)
- [操作符重载](#操作符重载)
- [集合](#集合)

## 简介

Groovy是一个通用的、面向对象的、基于JVM（Java虚拟机）的编程语言，编译成Java二进制代码。

官网：

[http://www.groovy-lang.org/](http://www.groovy-lang.org/)

下载：

[http://www.groovy-lang.org/download.html](http://www.groovy-lang.org/download.html)

w3cschool的Groovy教程：

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

## 基本语法

例子：

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

**注**：

- 分号是可选的，可不加。
- 括号也是可选的，也可以不加。
- Groovy中有两种类型的字符串：
  - 单引号字符串：是java字符串，java.lang.String的实例。
  - 双引号字符串：是Groovy字符串，允许插值篡改。

**包**

Groovy中会自动导入以下包：

- java.lang
- java.util
- java.io
- java.net
- groovy.lang
- groovy.util

**基本数据类型**

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

**注**：

- Groovy允许使用一个实际类型，如String、Date、Employee定义变量等，也可以使用def关键字。使用def关键字的是动态数据类型，可改变，而使用String等直接声明的是静态数据类型的。

- "hello,${a}"是字符串插值，完整形式。
- "hello,$a"也是符串插值，当没有歧义时，可以使用类似此种的简短形式。

## assert方法和Groovy真值

Groovy的**assert方法**是根据**Groovy真值**来评估参数的：

- 非零数字，如正、负数，是真
- 非空集合，如字符串，是真
- 非null的引用，是真
- Boolean类型的true，是真

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

**注**：

- instanceof运算符：该运算符是二目运算符，左边的操作元是一个对象，右边是一个类，当左边的对象是右边的类或子类创建的对象时，该运算符运算的结果是true，否则为false。
- Range包含两个值，用两个点号隔开，如a[0..2]。
- Maps使用冒号分隔键和值，Map上的方括号就是getAt或putAt方法。