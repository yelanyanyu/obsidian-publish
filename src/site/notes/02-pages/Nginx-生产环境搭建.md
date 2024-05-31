---
{"dg-publish":true,"permalink":"/02-pages/Nginx-生产环境搭建/","tags":["personal/blog","program/tech/nginx","program/tech/docker"]}
---

# Centos 7.6-Vmware测试环境
>Nginx 位置： `/usr/local/nginx` 目录下。
## 基本指令
```text
修改配置: $ vim /usr/local/nginx/conf/nginx.conf
启动: $ /usr/local/nginx/sbin/nginx -c nginx.conf
停止：$ /usr/local/nginx/sbin/nginx -s stop
检查配置文件: $ /usr/local/nginx/sbin/nginx -t
重新加载(不需要重启): $ /usr/local/nginx/sbin/nginx -s reload
查看版本: $ /usr/local/nginx/sbin/nginx -v
查看版本、配置参数: $ /usr/local/nginx/sbin/nginx -V
```

# docker 镜像配置-推荐
## 踩坑点
1. 在 root 目录下设置挂载点。请在根目录，也就是 `/` 目录下创建，而不要在 `/root` 中创建挂载点，不然会很容易报 403 forbidden，显示没有权限访问；

2. 没有关闭对应端口的防火墙。假如，我们设置了 docker 容器的端口映射为 `80:80`，那么我们就要确保防火墙的 80 端口是允许 tcp 连接的。即使用指令 `firewall-cmd --list-all`。出现如下结果：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/02/20240209175444.png)
## 基本指令
### 启动 docker nginx 容器
```bash
docker run -p 80:80 --name nginx \
-v /mydata/nginx/html:/usr/share/nginx/html \
-v /mydata/nginx/logs:/var/log/nginx \
-v /mydata/nginx/conf/:/etc/nginx \
-d nginx:1.10
```

```ad-note
title: 解读
1. `-d nginx:1.10`，我们指定了nginx的版本为1.10，其他版本也是通用的，如果只有一个 `-d nginx`就默认获取的是最新版的nginx。
2. `-v`，表示的挂载操作，通过挂载操作，我们就可以在宿主机上操作就相当于在容器中操作相关文件。
	1. 例如，当nginx尝试在 `/etc/nginx`文件下查找 `nginx.conf`文件时，就会到宿主机的 `/mydata/nginx/conf`目录下查找。所以，我们只需要在 `/mydata/nginx/conf`下配置好 `nginx.conf`即可；

```

### 关闭 nginx 容器
```bash
sudo docker stop nginx
```

### 启动与重启
启动 nginx。
```bash
sudo docker start nginx
sudo docker restart nginx
```

### 删除 nginx 容器
```bash
sudo docker stop nginx
sudo docker rm nginx
```

```ad-important
title: 请注意
在删除容器前请先关闭容器。也就是执行 `sudo docker stop nginx`.
```

## 基本配置
在 nginx docker 中，有两个配置目录，一个是 `conf.d` 文件夹，一个是 `nginx.conf` 文件。

我们通常在 `conf.d` 文件夹下配置 server，而在 `nginx.conf` 中配置其他。

# 注意事项
+ 配置文件不一定位于 `/usr/local/nginx/conf/`，也有可能位于 `/usr/local/nginx/` 目录下，再修改 nginx 配置文件后，请先执行命令 `/usr/local/nginx/sbin/nginx -V` 确定配置目录后再进行操作，以免进行无用功，出现明明 reload 确不生效的情况；

