# staffjoy

[git 仓库](https://github.com/oaol/fo-staffjoy)

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

> http://www.ruanyifeng.com/blog/2014/05/restful_api.html restful

> url 使用 “-” 分割

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

jar 包版本号统一管理 apply from: "config.gradle"

```java
ext {
    xxxVersion = "1.2.47"
}
```
> 必须使用双引号
implementation("com.alibaba:fastjson:${fastjsonVersion}")


### 多项目 mono project 单体仓库

1. settings.gradle
   1. include "shared", "api", "services:webservice", "services:shared"
2. 公共配置
3. Builder 泛型 SSSS.<String>builder().xx().build();

> use java.util.concurrent.TimeUnit
> Collections.unmodifiableMap(map)

## modules

1. common module

auth

- http header:  
  - cookie name
  - user head key
  - Authorization key
- query value from header
- auth interceptor with "Authorize" annotation
- custom permission denied exception with http status(@ResponseStatus(code=HttpStatus.xxx,reason="xxxx") on exception class)
- feign http request head witch user id
- sessions
  - loginUser, set cookie witch generated by "fo.staffjoy.common.crypto.Sign" class
  - getToken, get sessionToken from cookie
  - logout, set sessionToen maxAge to zero

config  

- StaffProps
- StaffConfig
  -  implement WebMvcConfigurer and override “addInterceptors” to add "AuthorizeInterceptor"
  -  init "EnvConfig" bean
  -  init "FeignRequestInterceptor"
- StaffjoyRestConfig
  - Use this common config for Rest API, import 
- StaffjoyWebConfigv
  - Use this common config for Web App, import

crypt  

- Hash
- Sign, jwt
  - algorithmMap
  - verifierMap
  - generateEmailConfirmationToken
  - verifyEmailConfirmationToken
  - verifySessionToken
  - generateSessionToken

env

- EnvConfig
  - name
  - debug
  - externalApex
  - internalApex
  - scheme
  - EnvConfig map
  - getEnvConfig from EnvConfig map by env
- EnvConstant
  - dev
  - test
  - uat
  - prod

error  

- gloal exception traslator

exception  

- custom runtime exceptions

services  

- Service
- SecurityConstant
- ServiceDirectory
  - service map

1. account module

## project

> 使用 spring ResponseEntity 做为结果集，使用 http 状态码

1. spring boot
2. spring cloud feign

> gradle build account api DTO error: cannt find symbol. 项目不是 springboot 项目

```
应用 Spring Boot 插件会导致问题,因为它正在禁用演示 api 的 jar 任务,其输出是编译项目(":demo-api]"添加到类路径的内容。您可以通过仅将弹簧引导插件应用于属于弹簧启动应用程序的项目或启用演示 api 项目中的 jar 任务来解决此问题。
不需要启动的项目设置 bootJar.enabled = false
```

> 之前测试成功了，gradle build 突然又出现了 cannot find symbol 错误，在 account-api 添加  bootJar.enabled = false
jar.enabled = true 

> gradle build 跳过 test gradle build -x  test

> gradle 执行 test, gradle test

> gradle build feign 模块报错，不能强转。
> 不能强转问题随着 cannot find symbol 的出现解决不存在了

```java
/Users/bryce/codes/ls/java/fo-staffjoy/feign-test/src/main/java/fo/staffjoy/feign/controller/TestController.java:21: error: incompatible types: TestDto cannot be converted to ResponseEntity<TestDto>
        ResponseEntity<TestDto> test = accountClient.test(name);
```


###  bot api

> 通过单元测试、集成测试来验证功能

> 是一个消息转发服务，它一方面作为队列可以缓冲高峰期的大量通知消息，另一方面作为代理可以屏蔽将来可能的通知方式的变更。

> 账号注册、添加员工、排班都需要发送邮件


#### mail

account 为什么会直接调用 mail

> spring 项目内异步处理

单元测试：  
controller. ResponseEntity 返回结果正确的验证： ResponseEntity.getStatusCode().is2xxSuccessful()
spring mockbean

配置域名
配置发信地址
在 aliyun 域名解析配置 txt 
查看域名配置 dig txt aliyundm.www.luosheng.tech

controller 层构造函数注入失败
#### sms

> 暂时不做

### Faraday 可编程网关

> 一个反向代理(功能类似nginx)，也可以看作是一个网关(功能类似zuul)，它是用户访问Staffjoy微服务应用的流量入口，它既实现对前端应用和后端API的路由访问，也实现登录鉴权和访问控制等安全功能。Faraday代理是Staffjoy微服务架构和前后分离架构的关键，并且它是唯一具有公网IP的服务。

1. 定义通用异常
2. 异常页面
3. 定义业务异常，展示异常页面
4. 捕获异常状态展示异常页面

controller

- GlobalErrorController, global error page

config

- FaradayProperties
  - MetricsProperties
  - TracingProperties
  - proxy mappings
- MappingProperties, proxy mapping properties
- MetricsProperties
- StaffjoyPropreties
- TracingProperties
  
  core

  - balancer
    - LoaderBalancer, @interface
    - RandomLoadBalancer, implements LoaderBalancer, java.util.concurrent.ThreadLocalRandom.current random

filter

- favicon filter, path /favicon return 200
- HealthCheckFilter, path /health return 200
- NakeDomainFilter, staffjoy.xyz/foo?true=1 should redirect to www.staffjoy.xyz/foo?true=1
- SecurityFilter ? 

http  

- ForwardDestination
- HttpClientProvider
  - httpClient map, update by MappingProperties
- RequestData
- RequestDataExtractor



### web app

> 是一个前端MVC应用，它主要支持产品营销、公司介绍和用户注册登录/登出，这个应用也称为营销站点(Marketing Site)或者登录页(Landing Page)应用

### who am i

> 支持前端应用获取当前登录用户的详情信息，包括公司和管理员身份，团队信息等，它也可以看作是一个用户会话(Session)信息服务。

### OCR

注册华为云账号

### account

账户服务，提供账户注册、登录认证和账户信息管理等基本功能

config  

- swagger, 之后修改为 

### company

## build

### DOCKERFILE

1 account

## CI/CD

### install jenkins

docker pull jenkins
docker run -d --name jenkins-dc -p 8079:8080 -p 50000:50000 -v /Users/bryce/docker/datas/jenkins:/var/jenkins_home jenkins

## problem

gradle 打包报错：  
Task :faraday:compileJava FAILED
/Users/bryce/codes/ls/java/fo-staffjoy/faraday/src/main/java/fo/staffjoy/faraday/core/mapping/MappingsValidator.java:4: error: package org.apache.commons.lang3 does not exist
import static org.apache.commons.lang3.StringUtils.isBlank;

move common-lib's dependencies to parent

RequestParam.value() was empty on parameter 1, spring cloud feign  @RequestParam should with value


cannot retry due to redirection, in streaming mode executing POST

cannot use base path as a request url. controller add a default path

Client use feign.httpclient.ApacheHttpClient

```
    implementation 'org.apache.httpcomponents:httpclient:4.5.9'
    implementation 'io.github.openfeign:feign-httpclient'
```

gradle test does not exist

> Task :account-svc:compileTestJava FAILED
/Users/bryce/codes/ls/java/fo-staffjoy/account-svc/src/test/java/tech/staffjoy/account/controller/AccountControllerTest.java:11: error: package org.apache.commons.lang does not exist
import org.apache.commons.lang.StringUtils;

and fix feign get Bean issue

TODO

### local ports

|service| port |
| --- | --- |
| mail | 8083 |
| faraday | 8080 |
| account | 8081 |
| web | 8086 |

###
- [gradle1](https://www.w3cschool.cn/gradle/9b5m1htc.html)
- [gradle2](https://www.tutorialspoint.com/gradle/)
- [gradle3](https://github.com/ksoichiro/awesome-gradle)
- [gradle portal](https://docs.gradle.org/current/userguide/intro_multi_project_builds.html)
- [simple demo](https://github.com/forjyoung/multi-gradle-test)
- [cannt find symbol](https://github.com/spring-projects/spring-boot/issues/11594)
- [spring additional config](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html)
- [aliyun mail](https://help.aliyun.com/document_detail/29459.html?spm=a2c4g.11186623.6.609.65f1778fWIROSf)
- [k8s](https://mp.weixin.qq.com/s/q8ic-Ddht04WU4z1aTc0iA)
