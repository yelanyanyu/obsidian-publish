---
{"dg-publish":true,"permalink":"/02-pages/docker desktop 更改容器端口映射/","tags":["personal/blog","program/tech/docker"]}
---

# 基本步骤
1. 停掉 docker 容器和 docker 服务进程；
2. 在文件管理中输入这个地址：**\\wsl. localhost\docker-desktop-data\data\docker\containers**。
3. 进入对应的容器 id 文件夹，修改 `hostconfig.json`；
4. 修改 `config.v2.json`；
5. 重启 docker desktop 和对应容器；

对于文件`hostconfig.json`，修改方式如下：
![image.png|800](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/06/20240609104209.png)

对于文件`config.v2.json`，修改如下：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/06/20240609104244.png)

之后的效果为：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/06/20240609104300.png)
