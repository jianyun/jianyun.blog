title: java类装载机制
date: 2017-08-07 19:55:39
tags:
category: 笔记
---
## 概述

Class文件由类装载器装载后，在JVM中将形成一份描述Class结构的元信息对象，通过该元信息对象可以获知Class的结构信息：如构造函数，属性和方法等，Java允许用户借由这个Class相关的元信息对象间接调用Class对象的功能。
虚拟机把描述类的数据从class文件加载到内存，并对数据进行校验，转换解析和初始化，最终形成可以被虚拟机直接使用的Java类型，这就是虚拟机的类加载机制。

类装载器就是寻找类的字节码文件，并构造出类在JVM内部表示的对象组件。在Java中，类装载器把一个类装入JVM中，要经过以下步骤：

 (1) 装载：查找和导入Class文件；

 (2) 链接：把类的二进制数据合并到JRE中；

    (a)校验：检查载入Class文件数据的正确性；

    (b)准备：给类的静态变量分配存储空间；

    (c)解析：将符号引用转成直接引用；

 (3) 初始化：对类的静态变量，静态代码块执行初始化操作

 ## 类加载器与双亲委派模型

    (1) Bootstrap ClassLoader : 将存放于<JAVA_HOME>\lib目录中的，或者被-Xbootclasspath参数所指定的路径中的，并且是虚拟机识别的（仅按照文件名识别，如 rt.jar 名字不符合的类库即使放在lib目录中也不会被加载）类库加载到虚拟机内存中。启动类加载器无法被Java程序直接引用
    (2) Extension ClassLoader : 将<JAVA_HOME>\lib\ext目录下的，或者被java.ext.dirs系统变量所指定的路径中的所有类库加载。开发者可以直接使用扩展类加载器。
    (3) Application ClassLoader : 负责加载用户类路径(ClassPath)上所指定的类库,开发者可直接使用。

 工作过程：如果一个类加载器接收到了类加载的请求，它首先把这个请求委托给他的父类加载器去完成，每个层次的类加载器都是如此，因此所有的加载请求都应该传送到顶层的启动类加载器中，只有当父加载器反馈自己无法完成这个加载请求（它在搜索范围中没有找到所需的类）时，子加载器才会尝试自己去加载。

 好处：java类随着它的类加载器一起具备了一种带有优先级的层次关系。例如类java.lang.Object，它存放在rt.jar中，无论哪个类加载器要加载这个类，最终都会委派给启动类加载器进行加载，因此Object类在程序的各种类加载器环境中都是同一个类。相反，如果用户自己写了一个名为java.lang.Object的类，并放在程序的Classpath中，那系统中将会出现多个不同的Object类，java类型体系中最基础的行为也无法保证，应用程序也会变得一片混乱。


## 反射

 Reflection机制允许程序在正在执行的过程中，利用Reflection APIs取得任何已知名称的类的内部信息，包括：package、 type parameters、 superclass、 implemented interfaces、 inner classes、 outer classes、 fields、 constructors、 methods、 modifiers等，并可以在执行的过程中，动态生成instances、变更fields内容或唤起methods。

* 1、获取构造方法
```
    Constructor getConstructor(Class[] params)
    根据构造函数的参数，返回一个具体的具有public属性的构造函数

    Constructor getConstructors()
    返回所有具有public属性的构造函数数组

    Constructor getDeclaredConstructor(Class[] params)
    根据构造函数的参数，返回一个具体的构造函数（不分public和非public属性）

    Constructor getDeclaredConstructors()
    返回该类中所有的构造函数数组（不分public和非public属性）
```

* 2、获取类的成员方法　
```
    Method getMethod(String name, Class[] params)
    根据方法名和参数，返回一个具体的具有public属性的方法

    Method[] getMethods()
    返回所有具有public属性的方法数组

    Method getDeclaredMethod(String name, Class[] params)
    根据方法名和参数，返回一个具体的方法（不分public和非public属性）

　　Method[] getDeclaredMethods()
    返回该类中的所有的方法数组（不分public和非public属性）
```

* 3、获取类的成员变量（成员属性）
```
    Field getField(String name)
    根据变量名，返回一个具体的具有public属性的成员变量

    Field[] getFields()
    返回具有public属性的成员变量的数组

    Field getDeclaredField(String name)
    根据变量名，返回一个成员变量（不分public和非public属性）

    Field[] getDelcaredFields()
    返回所有成员变量组成的数组（不分public和非public属性）
```