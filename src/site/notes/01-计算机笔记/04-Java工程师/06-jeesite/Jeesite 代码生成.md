---
{"dg-publish":true,"permalink":"/01-计算机笔记/04-Java工程师/06-jeesite/Jeesite 代码生成/","tags":["personal/blog","java","program/frontend/vue","program/backend/framework/springboot"]}
---

# 步骤概述
1. 启动 jeesite 后端模块 `web-api[jeesite-web-api]`；
2. 启动 jeesite-vue 项目；
3. 用 eclipse，配合官方插件打开 erm 文件，创建数据表（自己写 ddl 语句也可以，不过用该工具规范一些）；
4. 登录后，找到工具栏 `研发工具 -> 代码生成工具`，然后就可以根据提示生成代码了；
5. 配置相关信息；
6. 随后，生成的代码会自动出现在你的项目目录中；
7. 设置超级管理员权限；

# 具体步骤
## 数据表生成
这里仅仅只展示如何利用官方软件生存数据表。
[[01-计算机笔记/04-Java工程师/06-jeesite/Jeesite 自动生成 DDL\|Jeesite 自动生成 DDL]]
## 代码生成
当我们数据表成功创建，我们就可以尝试进行代码生成了。代码生成主要在平台上完成。请先保证数据表已经创建。
1. 打开前后端服务（一个是 jeesite-web-api，一个是 vue）；
2. 登录后，找到工具栏 `研发工具 -> 代码生成工具`，然后就可以根据提示生成代码了；
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240426194734.png)
3. 我们点击新增，后选择我们刚刚添加的表：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240427160212.png)
4. 填写好相关信息后，就可以生成代码了；
5. 配置相关信息，生成模版选择 vue，生成的后端路径选择 `jeesite-web-api` ，前端路径选择 `jeesite-vue` 的项目根目录；
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240426195145.png)
6. 设置管理员权限，让我们能够访问对应的内容：[[01-计算机笔记/04-Java工程师/06-jeesite/Jeesite 设置管理员权限\|Jeesite 设置管理员权限]]
# 注意事项
## 生成路径问题
我们可以选择在任意的位置生成代码，不过在实际生产中，我记录一下我使用的两种实践：
1. 在无关文件夹中生成，即新建一个文件夹，然后将代码直接生成在那个文件夹中。好处是，可以清晰的看到生成了什么东西。但是如果对项目结构不了解，那么可能会对使用一头雾水；
2. 在项目文件夹中生存，生存的代码可以直接投入测试使用。在有了可视化 git （例如 IDEA 的 git）的帮助后，我们也能清晰的查看新建了哪些文件；

## 树形表代码生成
有必要特意的注意树形表（例如分层表）的代码生成，这个需求在实际中运用还是很多的。
[[01-计算机笔记/04-Java工程师/06-jeesite/Jessite 树形表实例\|Jessite 树形表实例]]
