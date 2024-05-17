---
{"dg-publish":true,"permalink":"/01-计算机笔记/05-编程语言/01-Java基础/Java Immutable set/","tags":["personal/blog","java/collection","java/jdk9"]}
---

# What is immutable set
Immutable set（不可变集合），是在 JDK 9 引入的新特性，所以由工厂方法创建的集合都是不可变的，具体来说，例如 `of(...)` 方法创建的集合都是不可变的。
```java
Set<String> immutableSet = Set.of("Apple", "Banana", "Orange");
```

什么是不可变？就是只读的集合，类似与 python 的 tuple。一旦我们尝试使用集合的 `add` 的方法，那么就会报错 `UnsupportedOperationException`。