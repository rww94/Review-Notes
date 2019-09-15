<!-- TOC -->

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

<!-- /TOC -->

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

