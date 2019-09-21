<!-- TOC -->

- [1 static和final关键字](#1-static和final关键字)
    - [1.1 static](#11-static)
    - [1.2 final](#12-final)
- [2 接口和抽象类](#2-接口和抽象类)
- [3 equlas和hashcode](#3-equlas和hashcode)
- [4 hashMap](#4-hashmap)
- [5 java 容器类](#5-java-容器类)
    - [5.1 Collection](#51-collection)
    - [5.2 Map](#52-map)
- [6 强制类型转换](#6-强制类型转换)
    - [6.1 向上转换](#61-向上转换)
    - [6.2 向下转换](#62-向下转换)
- [7 java函数只有值传递没有引用传递](#7-java函数只有值传递没有引用传递)
- [8 String,StringBuffer,StringBuilder](#8-stringstringbufferstringbuilder)
- [9 Java创建对象的几种方式](#9-java创建对象的几种方式)
- [10 final、finally、finalize的区别](#10-finalfinallyfinalize的区别)
- [11 转发和重定向的区别](#11-转发和重定向的区别)
- [12 wait、sleep和notify的区别](#12-waitsleep和notify的区别)

<!-- /TOC -->
# 1 static和final关键字
## 1.1 static
static的意思是“静态的”，一般修饰属性和方法。
* 1、static作用于某个字段时，一个static字段对于每个类来说都只有一份存储空间，而非static字段是每个对象都有一份存储空间；
* 2、static作用于方法的用法是不用创建任何对象就能直接通过类名调用static方法；
* 3、static不能作用于局部变量
* 4、Java中禁止全局方法，所有引入static方法通过类本身调用；
* 5、static不能修饰普通类，但可以作用于内部类。
## 1.2 final
final意味着“不可改变”，一般应用于数据、方法和类。
* final数据：当数据是基本类型时，意味着这是一个永不改变的编译时常量，一个在运行时初始化的值，不希望他改变。
当数据是引用类型时，用static和final修饰表示这是一块不能改变的内存空间。
* final参数：当我们把方法传入的形参定义为final的时候，代表我们不想在方法内部修改此参数的引用；
事实是当我们使用不带有final的关键字时，函数内部的引用改变也不会对外界的实参产生影响，所以我们认为这里是编译器的阻拦，起到警示作用。
* final方法：我们使用final方法阻止子类对改类方法的修改或覆盖；
类中所有private方法都隐式的制定为final，因为private只在本类显示，即使是子类也不能操作该方法，
# 2 接口和抽象类
# 3 equlas和hashcode
# 4 hashMap
# 5 java 容器类
## 5.1 Collection
&emsp;&emsp;包括List和Set：List包括LinkedList、ArrayList、Vector,Stack继承Vector；Set包括HashSet，TreeSet，LinkedHashSet。
* LinkedList：基于链表实现的，不具有线程安全性，查询慢，增删快，效率高；
* ArrayList：基于数组实现的，不具有线程安全性，查询快，增删慢，效率高；
* Vector：基于数组实现，具有线程安全性，查询快，增删慢，效率低；
<br>

* HashSet：基于Hash表（数组+链表）实现的，元素没有排序顺序（顺序是不确定的），add()，remove()以及contains()等方法都是复杂度为O(1)的方法，；
* TreeSet：基于树结构（红黑树）实现的，元素是按顺序排列的(自然顺序)，add()，remove()以及contains()等方法的复杂度为O(log(n));
* LinkedHashSet：基于Hash表（数组+链表）实现的，介于HashSet和TreeSet之间，同时维护一个双链表来记录插入顺序，顺序是插入顺序；
## 5.2 Map
&emsp;&emsp;Map包括：HashTable，TreeMap，HashMap，LinkedHashMap继承HashMap。
* HashMap：基于hash表（数组+链表）实现的，元素没有顺序，不具有线程安全性，允许key值为null，初始化数组大小为16；
* LinkedHashMap：基于Hash表和双向链表实现的，元素按照插入顺序排序，不具有线程安全性；
* HashTable：基于Hash表实现，元素没有顺序，线程安全的；
* TreMap：基于红黑树，元素按照自然排序，线程安全。
# 6 强制类型转换
## 6.1 向上转换
对于基础类型可以自动转换，例如int转long，因为long的范围更大
```java
int a = 1;
long b = a;
```
对于对象，例如A是B的父类，下面就是父类对象指向子类的引用，调用a对象，实际上是B类
```java
A a = new B();
```
## 6.2 向下转换
&emsp;&emsp;向下类型转换是强制类型转换，通过（）符号进行转换，但会丢失精度，如果a超过了int的最大值，那么强转成int，只会等于int的最大值。
```java
long a = 10;
int b = (int)a;
```

# 7 java函数只有值传递没有引用传递
&emsp;&emsp;值传递：对所传递参数进行一次副本拷贝，对参数的修改只是对副本的修改，函数调用结束后，副本丢弃，原来的变量不变；
&emsp;&emsp;引用传递：参数被传递到函数时，不复制副本，而是直接将参数自身传入到函数中，函数内对参数的任何改变都将反应到原来的变量上。
* 对于基本类型，传递的是值，这个值是复制的一份，所以不会改变原值；
* 引用类型，传递的是地址，如果地址变了，那么原来的值肯定不变；（例如String，在函数中重新复制，引用地址变了，原来的值不变）
* 引用类型，传递的是地址，如果地址没变而改变了地址对应的对象的属性，那么会改变原来的值。
# 8 String,StringBuffer,StringBuilder

# 9 Java创建对象的几种方式
* 用new关键字；
* 调用对象的clone方法；
* 利用反射，调用class类或者Constructor类的newInstance()方法；
* 用反序列化，调用ObjectInputStream类的readObject()方法；
```java
public class TestClass implements Cloneable{
    @Override
    protected TestClass clone() throws CloneNotSupportedException {
        return (TestClass)super.clone();
    }
}
public static void main(String[] agrs){
    //方式1
    TestClass test = new TestClass();
    //方式2
    TestClass test1 = (TestClass)Class.forName("包名.TestClass").newInstance();
    //方式3
    try{
        TestClass test2 = (TestClass)test1.clone();  //要使用clone方法，实现Cloneable接口，重写clone方法，加异常处理
    }catch(CloneNotSupportedException e){
        e.printStackTrace();
    }
}
```
# 10 final、finally、finalize的区别
* final
修饰类，表明类不可继承
修饰方法，表明方法不能被重写
修饰变量，表明该变量不可修改，且只能赋值一次
* finally
finally是保证代码一定要被执行的一种机制
比如try-finally或try-catch-finally，用来关闭JDBC连接或者处理异常
* finalize
finalize是Object的一个方法，他的目的是保证对象在被垃圾回收前完成特定资源的回收

# 11 转发和重定向的区别
* 转发是一次请求，重定向是多次请求。
* 如果数据存在request对象中，由于转发是一次请求所以数据可以传递；而重定向是多次请求，所以是不同的请求，存储的request对象中的数据无法传递；如果数据存在session中，代表服务端和客户端的一次会话，包含多次请求和响应过程，所以不管是重定向还是转发都能传递数据。
* 如果想跳转到百度，需要使用重定向，因为转发的范围是在服务器内部。

# 12 wait、sleep和notify的区别
