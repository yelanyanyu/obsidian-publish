---
{"dg-publish":true,"permalink":"/01/04-java/06-jeesite/jeesite/","tags":["blog","jeesite"]}
---

# 超级管理员增加权限
```ad-note
在我们新增了一些前端以及后端页面，由于新增了一些 shiro 的鉴权逻辑，会导致我们无法正常访问新建的资源。所以，需要开发者手动增加超级管理员的权限。
```
1. 找到 `系统管理 -> 权限管理 -> 角色管理 -> 系统管理员`，如下图：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240427161453.png)
2. 在当前行 `授权菜单`增加该用户的权限：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240427161557.png)
3. 在 `数据权限` 中，选择全部数据：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/04/20240427161708.png)