---
{"dg-publish":true,"permalink":"/01-计算机笔记/04-Java工程师/99-bugs/001-maven/","tags":["personal/blog","program/bug"]}
---


# IDEA 的 maven 无法下载文档和源代码
解决办法，终端输入：
```bash
mvn dependency:resolve -Dclassifier=sources
```

# IDEA maven 提示包不存在
## 问题描述
明明包的内容都能够浏览，但是启动 maven 项目的时候就是报错 `com.xxx.xxx` 不存在，可以尝试一下做法：

## 问题解决
>出现 jar 包找不到的问题，首先有可能是项目依赖中有些 jar 没有下载完整，而 mvn idea: idea 这个命令可以检查并继续下载未下载完整的依赖 jar。在命令行输入 mvn idea: idea ，然后 file–invalidate caches 重启就可以了。操作如下图所示：![](https://img-blog.csdnimg.cn/5af3143d5757489dabe80f5348e339d5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQWxiZW5YaWU=,size_20,color_FFFFFF,t_70,g_se,x_16)
