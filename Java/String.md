# String类

目录：

- [构造字符串对象](#构造字符串对象)
  - [常量对象](#常量对象)
  - [字符串对象](#字符串对象)
  - [引用字符串常量对象](#引用字符串常量对象)
- [常用的方法](#常用的方法)
  - [length()](#length)
  - [equals()](#equals)
  - [equalsIgnoreCase()](#equalsIgnorecase)
  - [startsWith()](#startswith)
  - [endsWith()](#endswith)
  - [compareTo()](#compareto)
  - [contains()](#contains)
  - [indexOf()](#indexof)
  - [lastIndexOf()](#lastindexof)
  - [substring()](#substring)
  - [trim()](#trim)

String类为final类，因此不能扩展String类，String类不可以有子类。

## 构造字符串对象

### 常量对象

字符串常量对象用双引号括起的字符序列，如：“我”。

### 字符串对象

使用String类来声明，如：String a。

创建字符串对象：a=new String("abc");

使用已创建的字符串来创建另一个字符串，如：String b=new String(a);

使用一个字符数组来创建字符串对象，如：

```
char c[]= {'a','b','c'};
String d=new String(c);

同于下：

String d=new String("abc");
```
使用String(char[] value, int offset, int count)提取字符数组中的一部分字符来创建字符串对象，如：

```
String e=new String(c,1,2);

结果：
bc
```

offset为起始位置，count指个数。

### 引用字符串常量对象

可以把字符串常量的引用赋值给一个字符串变量，如：

```
String f1;
String f2;
f1="abc";
f2="abc";
```

“abc”是一个字符串常量。

因为引用相同，因此也具有相同实体。

注：但具有相同实体，引用不一定相同。

## 常用的方法

### length()

**public int length()**

用来获取字符串的长度，如：a.length()。

### equals()

**public boolean equals(Object anObject)**

比较当前字符串对象的实体是否与参数anObject指定的字符串的实体相同，如：f1.equals(f2)。

注：如：

```
String a=new String("abc");
String f1="abc";
String f2="abc";

System.out.println(f1.equals(f2));
System.out.println(f1==f2);

System.out.println(a.equals(f1));
System.out.println(a==f1);

输出结果：
true
true
true
false
```

因为**字符串**是对象，a中存放的是引用，所以a=="abc"为false。

### equalsIgnoreCase()

**public boolean equalsIgnoreCase(String anotherString)**

比较当前字符串对象与参数s指定的字符串是否相同，比较时忽略大小写，如：对于String a="abc"，a.equalsIgnoreCase("ABC")的结果为true。

### startsWith()

判断当前字符串对象的前缀是否与参数prefix指定的字符串相同。

**public boolean startsWith(String prefix)**

如：对于String a="abc"，a.startsWith("a")的结果为true。

**public boolean startsWith(String prefix,int toffset)**

参数toffset指从哪里开始寻找字符串，如：对于String a="abc"，a.startsWith("b", 1)的结果为true。

### endsWith()

**public boolean endsWith(suffix)**

判断一个字符串的后缀是否与参数prefix指定的字符串相同，如：对于String a="abc"，a.endsWith("c")的结果为true。

### compareTo()

**public int compareTo(String anotherString)**

字符串对象按字典序与参数anotherString指定的字符串比较大小，相同则返回0，当前字符串对象大于anotherString则返回正值，小于则返回负值，如：对于String a="abc"，a.compareTo("c")，结果为-2。

注：只是比较第一位。

**public int compareToIgnoreCase(String str)**

忽略大小写比较，如：对于String a="abc"，a.compareToIgnoreCase("C")的结果也为-2。

### contains()

**public boolean contains(CharSequence s)**

判断当前字符串对象是否含有参数s指定的字符串，如：对于String a="abc"，a.contains("c")的结果为true。

### indexOf()

**public int indexOf(String str)**

索引位置从0开始，从当前字符串头开始检索字符串str，并返回首次出现str的索引位置，如果没有检测到，则返回-1，如：对于String a="abc"，a.indexOf("c")的结果为2。

**public int indexOf(String str,int fromIndex)**

从当前字符串的fromIndex位置开始检索字符串str，对于String a="abc"来说，索引位置还是从0开始，字符c的索引位置还是2，如：对于String a="abc"，a.indexOf("c",2)的结果为2。

### lastIndexOf()

**public int lastIndexOf(String str)**

从当前字符串头开始检索字符串str，并返回最后出现str的索引位置，如：对于String a="abc dc"，a.lastIndexOf("c")的结果为5。

**public int lastIndexOf(String str,int fromIndex)**

从当前字符串头开始检索字符串str，检查字符长度为fromIndex，并返回最后出现str的索引位置，如：对于String a="abc dc"，a.lastIndexOf("c",6)，结果为5；a.lastIndexOf("c",3)，结果为2。

### substring()

**public String substring(int beginIndex)**

从当前字符串的beginIndex处截取到最后并得到当前字符串的子串，如：对于String a="abc dc"，a.substring(2)的结果为c dc。

**public String substring(int beginIndex,int endIndex)**

从当前字符串的beginIndex处截取到endIndex-1索引位置并得到当前字符串的子串，如：对于String a="abc dc"，a1.substring(2,5)的结果为c d。

### trim()

去掉当前字符串前后空格，如：对于String a="  abc  "，a.trim()的结果为abc。