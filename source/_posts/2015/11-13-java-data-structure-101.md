---
title: Java 数据结构 101
notebodate: 2015-11-13 12:00:00
categories:
- flow
tags:
- java
- data structure
---

## 原始类型及对象封装类

原始类型 | 封装类
---------|------:
boolean  |Boolean
char     |Character
byte     |Byte
short    |Short
int      |Integer
long     |Long
float    |Float
double   |Double

## 基础数据结构

线性表是最基本、最简单、也是最常用的一种数据结构。线性表中数据元素之间的关系是一对一的关系，即除了第一个和最后一个数据元素之外，其它数据元素都是首尾相接的。线性表有两种存储方式，一种是顺序存储结构，另一种是链式存储结构。

常用的数组就是一种典型的顺序存储结构。  
优点是查询的时间复杂度为O(1)，通过首地址和偏移量就可以直接访问到某元素，关于查找的适配算法很多，最快可以达到O(logn)。缺点是插入和删除的时间复杂度最坏能达到O(n)，且数组长度一旦声明便不可更改。
  
链表中两个相邻的元素在内存中可能不是相邻的，每一个元素都有一个指针域，指针域一般是存储着到下一个元素的指针。这种存储方式的优点是插入和删除的时间复杂度为O(1)，不会浪费太多内存，添加元素的时候才会申请内存，删除元素会释放内存，。缺点是访问的时间复杂度最坏为O(n)，关于查找的算法很少，一般只能遍历，这样时间复杂度也是线性（O(n)）的了,频繁的申请和释放内存也会消耗时间。

栈和队列是限制操作的线性表。

-   数组（array)
    -   [Java数组101](http://www.importnew.com/7127.html)
-   链表（list）
    -   [基本数据结构：链表（list）](http://www.cppblog.com/cxiaojia/archive/2012/07/31/185760.html)
    -   [数据结构之链表](http://www.cnblogs.com/wing011203/archive/2013/04/09/3010985.html)
-   栈（stack）
    -   [基本数据结构：栈（stack）](http://www.cppblog.com/cxiaojia/archive/2012/08/01/185913.html)
    -   [数据结构之栈和队列](http://www.cnblogs.com/wing011203/archive/2013/04/11/3014199.html)
-   队列（queue） 
    -   [基本数据结构：队列（queue）](http://www.cppblog.com/cxiaojia/archive/2012/08/02/186033.html) 
    -   [数据结构之栈和队列](http://www.cnblogs.com/wing011203/archive/2013/04/11/3014199.html)
-   二叉树（binary tree）
    -   [基本数据结构：树（tree）](http://www.cppblog.com/cxiaojia/archive/2012/08/03/186123.html)
    -   [基本数据结构：二叉树（binary tree）](http://www.cppblog.com/cxiaojia/archive/2012/08/06/186432.html)
    -   [java实现二叉树的构建以及3种遍历方法](http://ocaicai.iteye.com/blog/1047397)
    -   [数据结构之二叉树](http://www.cnblogs.com/wing011203/archive/2013/04/12/3016409.html)
    -   [二叉查找树（BST）实现](http://my.oschina.net/indestiny/blog/208297)
    -   平衡二叉树
    -   红黑树
-   哈希表(hashmap)
    -   [哈希表](http://www.cnblogs.com/jiewei915/archive/2010/08/09/1796042.html)

### [集合类](http://android.blog.51cto.com/268543/400557/)

Java Collections Framework是Java提供的对集合进行定义，操作，和管理的包含一组接口，类的体系结构。
 
Java集合框架的基本接口/类层次结构：

![Java Collections Framework](collections.png)

```
java.util.Collection [I]
+--java.util.List [I]
    +--java.util.ArrayList [C]
    +--java.util.LinkedList [C]
    +--java.util.Vector [C]
        +--java.util.Stack [C]
+--java.util.Set [I]
    +--java.util.HashSet [C]
    +--java.util.SortedSet [I]
         +--java.util.TreeSet [C]

java.util.Map [I]
+--java.util.SortedMap [I]
     +--java.util.TreeMap [C]
+--java.util.Hashtable [C]
+--java.util.HashMap [C]
+--java.util.LinkedHashMap [C]
+--java.util.WeakHashMap [C]
 
[I]：接口
[C]：类
```
