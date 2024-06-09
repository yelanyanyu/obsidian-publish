---
{"dg-publish":true,"permalink":"/02-pages/centos7 安装docker/","tags":["personal/blog","program/tech/docker"]}
---

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
