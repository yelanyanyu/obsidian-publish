---
{"dg-publish":true,"permalink":"/02-pages/'dockerHttpClient' should be set.Falling back to Jersey/","tags":["personal/blog","program/bug","program/tech/docker"]}
---

['dockerHttpClient' should be set.Falling back to Jersey · Issue #2352 · docker-java/docker-java](https://github.com/docker-java/docker-java/issues/2352)

Hello,

I ran into the same warning. However I did not get the Exception.

I maneged to get rid of the warning byI following the (brief) docs here on github and reached this solution:

```java
try(DockerClient dockerClient = openDockerClient()){
      // your  code
}
```

And the metod then:

```java
    private static DockerClient openDockerClient() {
        var config = DefaultDockerClientConfig.createDefaultConfigBuilder().build();
        DockerHttpClient httpClient = new ApacheDockerHttpClient.Builder()
                .dockerHost(config.getDockerHost())
                .sslConfig(config.getSSLConfig())
                .maxConnections(100)
                .connectionTimeout(Duration.ofSeconds(30))
                .responseTimeout(Duration.ofSeconds(45))
                .build();

        return DockerClientImpl.getInstance(config, httpClient);
    }
```

you also need to add this dependency with the client to your pom.xml:

```xml
        <dependency>
            <groupId>com.github.docker-java</groupId>
            <artifactId>docker-java-transport-httpclient5</artifactId>
            <version>3.4.0</version>
        </dependency>
```

This has removed the warning, uses the Apache HttpClient 5 and most importantly, it works. So hopefully this is the intended way.

Hope it helps.