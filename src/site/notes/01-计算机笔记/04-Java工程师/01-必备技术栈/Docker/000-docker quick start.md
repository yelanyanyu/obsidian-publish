---
{"dg-publish":true,"permalink":"/01-计算机笔记/04-Java工程师/01-必备技术栈/Docker/000-docker quick start/","tags":["personal/blog","#program/tech/docker"]}
---

# Docker install
## Uninstall old versions
```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
## Set up the repository
```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
## Install Docker Engine
```bash
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
## Start Docker
```bash
sudo systemctl start docker
```
## Run test program
```bash
sudo docker run hello-world
```
```ad-note
title:Optional
title:Optional
如果程序出现如下的结果就代表安装成功：
![](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2023/12/20231226224010.png)
```
## 设置自启动
```bash
sudo systemctl enable docker
```
## Install Aliyun mirror
```bash
sudo mkdir -p /etc/docker 
sudo tee /etc/docker/daemon.json <<-'EOF' 
{ 
	"registry-mirrors": ["https://xxx.mirror.aliyuncs.com"] 
} 
EOF 
sudo systemctl daemon-reload 
sudo systemctl restart docker
```

# Docker 常用操作
## 安装远程镜像
```bash
sudo docker pull [mysql:5.7]
```

## 查看已安装镜像
```bash
sudo docker images
```

## 启动镜像
我们以 mysql 为例，假如我们是刚刚配置 mysql。那么，在第一次启动的时候，需要进行配置：
```bash
sudo docker run -p 3306:3306 --name mysql \
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

# 常用问题汇总
## 01

## 01
