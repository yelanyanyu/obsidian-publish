---
{"dg-publish":true,"permalink":"/01/04-java/99-bugs/j-unit/","tags":["blog","bug"]}
---


# 使用 JUnit 测试多线程程序异常结束
JUnit 在主线程运行完毕后，会自动执行 `System.exit()` 方法，退出 JVM，如果有其他的子线程在运行，就会被立即停止。

解决方法：不要使用 JUnit。