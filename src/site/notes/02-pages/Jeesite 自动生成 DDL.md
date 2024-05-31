---
{"dg-publish":true,"permalink":"/02-pages/Jeesite 自动生成 DDL/","tags":["personal/blog","program/backend/framework/jeesite"]}
---

这里仅介绍如何使用 eclipse 的插件来生成表，table。
1. 首先要更新 eclipse 到新版；
2. 下载插件，这里手册有，就不赘述了；
3. 将 `test.erm` 文件由 eclipse 打开，该文件的位置如下：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240427104152.png)
4. 按照如下进行操作：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240427104310.png)
5. 随后就出现了创建表的窗体：
	我们可以选择相应的属性组，也可以自己新建字段。
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240427104345.png)
6. 创建完表后，我们右键空白处导出即可：
	这里有多个选项，我个人推荐导出 DDL 语句，这样定制化的程度高一点。
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240427104519.png)
