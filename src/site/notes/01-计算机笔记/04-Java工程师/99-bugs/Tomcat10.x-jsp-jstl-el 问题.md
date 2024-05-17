---
{"dg-publish":true,"permalink":"/01-计算机笔记/04-Java工程师/99-bugs/Tomcat10.x-jsp-jstl-el 问题/","tags":["personal/blog","program/bug","program/backend/framework/jsp","program/backend/framework/tomcat"]}
---

# 问题描述
在 Tomcat 升级到 10 后，由于从 `javax.` 升级到了 `jakara.` 会导致各种版本兼容问题的出现。如果要使用 jsp，以及 el 表达式，请如下进行配置。

# 问题解决
## 依赖更换
```xml
<!-- https://mvnrepository.com/artifact/jakarta.servlet/jakarta.servlet-api -->
<dependency>
	<groupId>jakarta.servlet</groupId>
	<artifactId>jakarta.servlet-api</artifactId>
	<version>6.0.0</version>
	<scope>provided</scope>
</dependency>
<!-- https://mvnrepository.com/artifact/jakarta.servlet.jsp.jstl/jakarta.servlet.jsp.jstl-api -->
<dependency>
	<groupId>jakarta.servlet.jsp.jstl</groupId>
	<artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
	<version>3.0.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.glassfish.web/jakarta.servlet.jsp.jstl -->
<dependency>
	<groupId>org.glassfish.web</groupId>
	<artifactId>jakarta.servlet.jsp.jstl</artifactId>
	<version>3.0.1</version>
</dependency>

```

## jsp 页面更改 prefix
```jsp
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jstl/core_rt" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
```
