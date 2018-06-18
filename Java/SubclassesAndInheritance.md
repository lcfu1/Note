# 子类与继承

目录：

- [子类与父类](#子类与父类)
- [子类的继承性](#子类的继承性)
- [子类与对象](#子类与对象)
- [例子](#例子)

## 子类与父类

由继承而来的类就是子类，而被继承的类则为父类。

子类不可以多重继承，也就是说子类只有一个父类。

子类可以继承父类的成员变量和方法。

所有类都是Object类的子孙类，即Object类是所有类的祖先类。

每个类（除了Object类）只有一个父类，而一个类可以有多个子类。

类中没有使用extends关键字，系统默认是Object的子类，`class A` 与 `class A extends Object`是一样的。

## 子类的继承性

子类可以继承父类的成员变量和方法，就好像在子类中直接声明一样。

在同一包中时，子类继承父类中不是`private`的成员变量和方法，且访问权限也不变。

不在同一包中时，子类继承父类中`protected`和`public`的成员变量和方法。

## 子类与对象

子类不继承父类的`private`成员变量，但子类创建一个对象时同样会给它分配内存空间，子类继承父类的方法中可能会使用这些变量，所以这些变量所分配的内存并不是垃圾。

## 例子

A.java

```
package Subclasses;

public class A {
//	private int a=1;       //私有变量，只能在这个类中使用
//	protected int a=1;	   //受保护变量，可被继承
//	public int a=1;		   //共有变量，可被继承
	int a=1;			   //友好变量，在同一包中可被继承，不在同一包中不可被继承
	void f() {
		System.out.println(a);
	}
}
```

B.java

```
package Subclasses;

public class B extends A{
	
}
```

C.java

```
package Subclasses;

public class C extends B{

}
```

main.java

```
package Subclasses;

public class main {
	public static void main(String args[]) {
		A a=new A();
		a.f();
		B b=new B();
		b.a=2;
		b.f();
		C c=new C();
		c.a=3;
		c.f();
	}
}
```

解析：

- A类是B类的父类，B类是C类的父类，C类是A类的子孙类。
- 在主类中创建A、B、C类的对象，B、C类的对象可以使用A类中的变量a和方法f()。