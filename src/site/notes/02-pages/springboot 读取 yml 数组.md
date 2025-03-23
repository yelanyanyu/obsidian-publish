---
{"dg-publish":true,"permalink":"/02-pages/springboot 读取 yml 数组/","tags":["personal/blog","program/backend/framework/springboot"]}
---

一直都在用 Spring 的@Value 注解读取 yml 中的配置，这两天在读取配置的时候，想读取 yml 中配置的一个数组，通过@Vaule 一直获取不到，通过一番资料的查询，才彻底清楚了@Vaule 的使用情况。

在 Spring 中读取配置文件的快捷方法常见的有两种，一个是通过@Vaule 注解进行单一字段的注入，另外一种方法就是通过@ConfigurationProperties 注解来进行批量注入。

> @ConfigurationProperties 注解属于 SpringBoot，不在 SpringFramework 里面

这两种注入方式各有自己的优势和使用场景。

|                        | @Value           | @ConfigurationProperties |
| ---------------------- | ---------------- | ------------------------ |
| 使用场景                   | 单一属性注入，注解写在类的属性上 | 批量注入，注解写在类上              |
| 松散语法                   | 不支持              | 支持                       |
| SpEL                   | 支持               | 不支持                      |
| JSR 303 数据校验@Validated | 不支持              | 支持                       |
| 复杂类型封装（数组、Map、对象等）     | ~~不支持~~ （这么说不严谨） | 支持                       |
|                        |                  |                          |

> 其实@Value 可以注入任意类型对象，数组、Map、List、自定义对象等。  
> 可以看我对@Value 的注入过程进一步的分析 [Spring的@Value可以注入复杂类型吗？今天教你通过@value注入自定义类型](https://blog.csdn.net/liujianyangbj/article/details/111352703)

yml 配置文件：

```c
test:
  list:
    - 'a'
    - 'b'
    - 'c'

```

数组、Map 等都输入复杂类型封装，Value 注解无法直接读取。  
但是可以通过@ConfigurationProperties 注解读取

#### 通过@ConfigurationProperties 注解读取

这里分为两种情况  
1、读取的是配置在 application. yml 文件中的属性  
只需要在类上加上注解就可以，配置好前缀

```java
@Component
@ConfigurationProperties(prefix = "test")
public class TestYML {

	private String[] list;

	public void test(){
		System.out.println("list:"+list);
	}

	///  set方法不能少
	public void setList(String[] list) {
		this.list = list;
	}
}

```

2、如果配置是在一个单独的 yml 文件中，例如 a.yml  
那么此时还应该加上一个@PropertySource 注解，指明来自哪个配置文件和一个 Factory 类

```java
@Component

@PropertySource(value = {"classpath:a.yml"}, factory = YamlPropertySourceFactory.class)
@ConfigurationProperties(prefix = "test")
public class TestYML {

	private String[] list;
	

	public void test(){
		System.out.println("list:"+list);
		
	}
	///  set方法不能少
	public String[] getList() {
		return list;
	}

}

12345678910111213141516171819
```
```java
import org.springframework.boot.env.YamlPropertySourceLoader;
import org.springframework.core.env.PropertySource;
import org.springframework.core.io.support.DefaultPropertySourceFactory;
import org.springframework.core.io.support.EncodedResource;

import java.io.IOException;
import java.util.List;

/**
 * @author KinYang.Lau
 * 用于读取 yml 类型的文件
 * @date 2020/9/26 7:06 下午
 */
public class YamlPropertySourceFactory extends DefaultPropertySourceFactory {
	@Override
	public PropertySource<?> createPropertySource(String name, EncodedResource resource) throws IOException {
		if (resource == null) {
			return super.createPropertySource(name, resource);
		}
		List<PropertySource<?>> sources = new YamlPropertySourceLoader().load(resource.getResource().getFilename(), resource.getResource());
		return sources.get(0);
	}
}

1234567891011121314151617181920212223
```

> **同时还有注意，要有 set 方法!!!**

#### 通过@Value 注解变相读取，曲线救国方案

因为@Value 注解是支持 SpEL 表达式的，所以可以在 yml 配置文件中，把之前的数组形式改写为由字符串形式，例如“a.b.c”

yml 文件内容

```java
test:
  list: a.b.c

12
```
```java
@Value("#{'${test.list}'.split('.')}")
private String[] list2;

12
```

这种方法是最简单的，不需要单独写一个类，不用 set 方法。  
如果 yml 是单独的文件的话，只需要在类上添加 `@PropertySource(value = "classpath:a.yml")` 注解就行。

---

注意： 有小伙伴反映，@Value 也可以直接注册数组或者结合。后来我测试了一下，发现当字符串是以, 分割的时候，就可以注入数组或者其他集合。 Spring 默认情况下会以“,”进行分割，转换成对应的数组或列表。感谢@丁乾文小伙伴的指出问题。