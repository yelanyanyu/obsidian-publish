---
{"dg-publish":true,"permalink":"/02-pages/CloudFlare 域名重定向次数过多/","tags":["personal/blog","program/bug"]}
---

这其实是Cloudflare为添加的站点加密模式设置错误导致的。

进入 Cloudflare Dashboard，点击有问题的域名，打开左侧的 SSL/TLS 设置，在 Overview 中设置加密模式为完全或完全（严格）即可。

![image-20231114230301888](https://i-blog.csdnimg.cn/blog_migrate/751b79f76b552387c850730abbb4fb8d.png)

这样子之后你的`vercel`应用应该是可以正常的在国内进行访问了。