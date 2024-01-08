---
{"dg-publish":true,"permalink":"/01/02-labs/00/000-vscode-c-c/","tags":["blog"]}
---

# 引言
至于下载安装 c、c++的插件之类的这里就不再赘述了。这里仅就配置过程可能遇到的困难进行处理。

# vscode 终端乱码
## 原因
通常是因为 vscode 使用的终端默认的编码格式不是 utf-8.

## 解决
[永久解决VS Code终端中文乱码问题_vs code中文乱码-CSDN博客](https://blog.csdn.net/lzyws739307453/article/details/89823900)
1. 打开 vscode 配置文件 `setting.json`，在设置中搜索然后打开即可：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108153733.png)
2. 在配置中加入如下片段：
```json
"terminal.integrated.profiles.windows": {
    "Command Prompt": {
        "path": "C:\\Windows\\System32\\cmd.exe",
        "args": ["-NoExit", "/K", "chcp 65001"]
    },
    "PowerShell": {
        "source": "PowerShell",
        "args": ["-NoExit", "/C", "chcp 65001"]
    }
},
"terminal.integrated.defaultProfile.windows": "Command Prompt"
```


# 头文件引用报错
在 windows 上，使用 vscode 运行 c/c++ 语言程序的时候，头文件报错如下：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108144403.png)

当我们刚配置完 vscode 初始环境后，会出现如下报错，提示我们更新 include path。
## 原因
原因很简单，是 vscode 并没有找到对应的头文件位置，我们只需要打开 c/c++配置进行设置即可。
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108144554.png)

我们要设置的是这个属性：
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108144810.png)


## 解决
1. 得到 gcc 的 include 文件：
	1. 打开终端，输入如下命令：`gcc -v -E -x c++ -`（c++配置输这个）或者 `gcc -v -E -x c -`，得到的结果如下：
		![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108154128.png)
		以上是 c 语言的结果，以下是 c++的结果：
		![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108162104.png)
	2. 将打钩的三个信息复制到 vscode 中的 include path 中，即可：
		![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108154329.png)
2. 刷新文件，提示正常：
	![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108154256.png)

# 点击运行显示 Permission Denied
## 问题描述
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108160133.png)

## 原因
这是由于该程序已经有进程在使用了，windows 10 不允许这样的行为。
## 解决
只用在任务管理器中 kill 掉这个进程即可。
![image.png](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/01/20240108160344.png)
