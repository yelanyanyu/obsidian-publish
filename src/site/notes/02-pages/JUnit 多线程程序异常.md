---
{"dg-publish":true,"permalink":"/02-pages/JUnit 多线程程序异常/","tags":["personal/blog","program/bug","algorithm/多线程","java"]}
---


# 使用 JUnit 测试多线程程序异常结束
JUnit 在主线程运行完毕后，会自动执行 `System.exit()` 方法，退出 JVM，如果有其他的子线程在运行，就会被立即停止。

## 解决方法 
不要使用 JUnit 进行多线程程序的单元测试，即不要在 Junit 单元测试中创建线程。