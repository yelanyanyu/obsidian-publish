---
{"dg-publish":true,"permalink":"/02-pages/docker quick start/","tags":["personal/blog","#program/tech/docker"]}
---

# Docker install
[[02-pages/centos7 安装docker\|centos7 安装docker]]
# Docker 常用操作
[[02-pages/docker 安装远程镜像\|docker 安装远程镜像]]
[[02-pages/docker 查看已安装镜像\|docker 查看已安装镜像]]
## 启动镜像
我们以 mysql 为例，假如我们是刚刚配置 mysql。那么，在第一次启动的时候，需要进行配置：
```bash
sudo docker run \
-p 3306:3306 \
--name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql:5.7
```

```ad-note
1. 3306:3306，表示的是主机的3306端口映射到容器的3306端口，我们可以通过主机的3306端口来操作容器的3306端口，启动mysql；
2. --name：容器的名字，这个由用户自行指定，只要不重复就行；
3. -v 三行：文件映射，即当我们操作主机的文件的时候，就相当于操作了容器中的文件；
4. -e和-d：指定mysql的用户和密码；
```

当第一次配置完毕后就不用再进行配置了，只需要运行即可：
```bash
sudo docker start mysql
```

## 运行容器
```bash
sudo docker exec -it mysql /bin/bash
```

## 重启/关闭/启动容器
```bash
sudo docker [restart|stop|start] mysql
```
