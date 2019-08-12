# staffjoy gradle version

## gradle 项目构建

###

> java 插件
> apply plugin: 'java'
> 提供了诸如编译，测试，打包等一些功能

> gradle tasks 
> 列出所有 task

> src/main/java 搜寻打包源码。
> src/test/java 下搜寻测试源码。
> src/main/resources 下的所有文件按都会被打包
> 所有 src/test/resources 下的文件 都会被添加到类路径用以执行测试。
> 所有文件都输出到 build 下，打包的文件输出到 build/libs 下。

```

project  
    +build  
    +src/main/java  
    +src/main/resources  
    +src/test/java 
    +src/test/resources

```

> gradle check
> Code-quality 插件

- 添加 maven 仓库

```

build.gradle

repositories {
    mavenCentral()
}

```

- 自定义 MANIFEST.MF

> gradle properties

```
sourceCompatibility = 1.5
version = '1.0'
jar {
    manifest {
        attributes 'Implementation-Title': 'Gradle Quickstart', 'Implementation-Version': version
    }
}

```

```lua
apply plugin: 'java'
apply plugin: 'eclipse'
sourceCompatibility = 1.5
version = '1.0'
jar {
    manifest {
        attributes 'Implementation-Title': 'Gradle Quickstart', 'Implementation-Version': version
    }
}
repositories {
    mavenCentral()
}
dependencies {
    compile group: 'commons-collections', name: 'commons-collections', version: '3.2'
    testCompile group: 'junit', name: 'junit', version: '4.+'
}
test {
    systemProperties 'property': 'value'
}
uploadArchives {
    repositories {
       flatDir {
           dirs 'repos'
       }
    }
}
```

### 多项目 mono project 单体仓库

1. settings.gradle
   1. include "shared", "api", "services:webservice", "services:shared"
2. 公共配置


## modules

1. common module
2. account module

## project

> 使用 spring ResponseEntity 做为结果集，使用 http 状态码

1. spring boot
2. spring cloud feign

> gradle build account api DTO error: cannt find symbol. 项目不是 springboot 项目

```
应用 Spring Boot 插件会导致问题,因为它正在禁用演示 api 的 jar 任务,其输出是编译项目(":demo-api]"添加到类路径的内容。您可以通过仅将弹簧引导插件应用于属于弹簧启动应用程序的项目或启用演示 api 项目中的 jar 任务来解决此问题。
不需要启动的项目设置 bootJar.enabled = false
```

> gradle build 跳过 test gradle build -x  test

> gradle 执行 test, gradle test

> gradle build feign 模块报错，不能强转。

```java
/Users/bryce/codes/ls/java/fo-staffjoy/feign-test/src/main/java/fo/staffjoy/feign/controller/TestController.java:21: error: incompatible types: TestDto cannot be converted to ResponseEntity<TestDto>
        ResponseEntity<TestDto> test = accountClient.test(name);
```


- [gradle1](https://www.w3cschool.cn/gradle/9b5m1htc.html)
- [gradle2](https://www.tutorialspoint.com/gradle/)
- [gradle3](https://github.com/ksoichiro/awesome-gradle)
- [gradle portal](https://docs.gradle.org/current/userguide/intro_multi_project_builds.html)
- [simple demo](https://github.com/forjyoung/multi-gradle-test)
- [cannt find symbol](https://github.com/spring-projects/spring-boot/issues/11594)