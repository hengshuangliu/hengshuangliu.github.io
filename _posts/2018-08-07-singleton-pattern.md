---
layout: post
title: Singleton pattern
categories: c++
description: Singleton单例模式的优化实现。
keywords: Singleton, 单例模式，c++
---
设计模式能让代码更好地维护。单例模式能够保证一个类仅有唯一的实例，并提供一个全局访问点。这种“单一职责”和“对象自治”的思想体现了面向对象中封装的概念。通常情况下，单例模式常应用于配置信息类、日志类、控制类、门面类、管理类和代理类等在整个软件系统中扮演协调作用的类。

## Singleton要点
1. Define a private static attribute in the "single instance" class；
2. Define a public static accessor function in the class;
3. Do "lazy initialization" (creation on first use) in the accessor function;
4. Define all constructors to be protected or private;
5. Clients may only use the accessor function to manipulate the Singleton;

## Singleton示例
```c++
class Singleton {
    public:
        static Singleton& Instance() {
            static Singleton theSingleton; //引用的好处是不需要判定指针，让Singleton实例自己调用析构函数销毁；
            return theSingleton;
        }
    
    private:
        Singleton();               // ctor hidden
        Singleton(Singleton const&);            // copy ctor hidden
        Singleton& operator=(Singleton const&); // assign op. hidden
        ~Singleton();                           // dtor hidden
};
```
* 任意两个singleton类的Instance函数不能互相引用对方的实例，会导致程序崩溃；
* 多线程情况下，可能存在两个线程同时调用创建方法，此时由于都未检测到实例，会各自创建一个实例，与原先意图违背。解决办法可以是在检测是否实例化时提供一个互斥锁；

## 后话

## 参考
* <https://www.cnblogs.com/zlcxbb/p/6795106.html>