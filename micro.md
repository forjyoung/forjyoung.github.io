# springboot & k8s

> staffjoy springboot 微服务 云原生 云容器 saas k8s DevOps
> 需求 架构设计 框架搭建 测试 可运维架构 容器云部署

## github 微服务案例

- eShopOnContainers 微软
- Microservices-Demo google
- Piggy-Metrics 个人 理财
- staffjoy 工时排班

## 1

## 2 架构设计和技术栈选型

- skywalking 调用链埋点监控
- bot 消息转发服务
- faraday 反向代理、网关

- 分而治之
- 单一职责
- 关注分离

本地 docker compose

> 多租户saas

## 3 数据和接口模型设计：帐户服务

intercom 客服系统

## 4 数据和接口模型设计：公司服务

## 5  Dubbo、Spring Cloud 和 Kubernets 技术选型

springboot + Kubernets
底层基础设施（公共关注点）- 微服务开发平台
k8s 在平台层解决服务发现和LB问题

## 实践框架 

springboot 的封装
微服务但是组织到一个仓库中：单体仓库 Mono-Repo。 shippable 的微服务之道

2. 微服务接口参数校验
3. 统一异常处理
4. DTO DMO 互相转化 https://github.com/modelmapper...
5. 接口和实现层分别创建项目
6. 强类型接口弱类型接口，结合 spring feign，动态代理
7. 分环境配置,sentry 异常日志
8. 异步调用处理(进程内异步调用), context 同步 登陆信息 线程调用链信息
9. swagger, openapi 规范。ascci 代替

## 可编程网关

> BFF backend for frentend

> frontend -> gateway -> BFF -> micro

### 反向代理和网关

统一网关

### 统一网关

faraday 简单网关

可优化
- 限流熔断
- 动态路由和负载均衡
- 基于 path 的路由
- 截获器链
- 日志采集和 metrics 埋点
- 响应流优化

主流网关
- nginx
- kong
- zuul
- spring cloud gateway
- envoy
- traefik

## 微服务安全

### 认证、授权

1. 认证， who am i 
2. 授权，有哪些权限

单页应用：
1. cookie + session

session 用户登陆状态的存储机制，http无状态，两次 http 

### 粘性会话

nginx 支持

1. 会话同步复制
2. 无状态会话，会话状态存储到浏览器端
3. 集中状态会话

### 微服务认证、授权

SSO single sign on

auth service
token
引入网关，认证授权在网关上处理

JWT
1. 安全不敏感
2. 透明令牌
3. 自包含，思想来zi于无状态会话
4. RFC7519
5. 授权及信息交换
6. header.payload.signature base64
7. jwt.io
8. hmac rsa

> jwt 优劣：
>> 紧凑轻量，对 auth server 压力小，简化 auth server 实现
>>无状态和吊销不能两全，传输开销

staffjoy:  
1. 登陆认证
2. 服务间调用鉴权，控制器截获鉴权机制
3. 用户角色鉴权, RBAC service

## 6 微服务测试


胖领域模型，瘦领域模型 domain 是否包含业务逻辑

服务代理 proxy，访问其他服务。反射加 httpclient

> 单元测试
> 集成测试
> 组件测试
> 契约测试 https://spring.io/projects/spring-cloud-contract pact
> 端到端测试 selenuim http://rest-assured.io/

测试金字塔

end to end 最佳实践

### staffjoy

单元测试单独的配置文件

## 分环境
环境变量

## 日志

结构化日志、审计日志

## 容器化和 docker compose 部署

本地开发环境
switch hosts
skywalking

前台项目两阶段构建
使用 .env 配置私密数据
compose 部署可以统一默认端口为 80
docker 访问本地数据库 mysql://host.docker.internal:prot/xx

## k8s 环境部署

集群
容器调度平台
pod > 容器
副本集 replicaSet
service,通过服务名寻址
deployment,发布
configMap,可变配置
secret
deamon set

### k8s 网络

节点网络， 节点之间通信寻址
pod 网络，内容器共享网络栈
service 网络
nodepod, kube proxy
ingress

### 金丝雀发布

先发布一个实例

### ci/cd

jenkins github/gitlab k8s 持续集成

## 分布式事务

### Distributed Transaction(two phase commit)

### Saga

每项微服务都有自己的私有数据，不能使用传统的分布式事务（JTA/Raft等两段提交PC）

## 扩展

1. https
2. 测试
3. ci/cd, jenkins
4. 监控
5. 帮助文档


## talk 

本课程案例相对简单，数据库表不多，不涉及分布式事务。关于分布式事务一致性，推荐参考dzone上的文章《Data Consistency in Microservices Architecture》(https://dzone.com/articles/data-consistency-in-microservices-architecture)。另外阿里开源的分布式事务组件seata(https://github.com/seata/seata)社区很热，可以参考。

stackflow上有个贴，讲java时间类的差异和用法，配图，可以参考：       
https://stackoverflow.com/questions/32437550/whats-the-difference-between-instant-and-localdatetime

为何不用Spring 事件监听模式，结合spring的异步去使用，而是自己去维护调用上下文？

你好，Idea里头报错是因为没有启用lombok，请启用一下，方法可以在网上搜一下。我的微信号:bulldog2015，加微信说明来自极客时间。

ReverseProxyFilter通过FilterRegistrationBean注册到Spring容器环境中，Spring会自动将这个Filter注册到Web容器中。参考faraday项目源码中的config/FaradayConfiguration这个Bean配置文件。

在 jwt 的公私钥模式中，私钥存在 Auth 服务器上，不能存客户端，公钥一般也不存客户端(存网关或者后台服务器上)，即使存客户端也没有大关系，公钥本来就是公开的，只不过能够解签jwt，不能篡改数据。

要想自动记录审计，可以参考一下HIbernate的Envers模块

 审计日志一般和重要的业务操作有关，可以手工埋点，实际也不是特别费事。当然，你可以考虑Spring AOP方式截获业务操作自动埋点，比如对Service或者Repository层通过aop截获写审计日志。

Staffjoy教学版依赖一些私密配置，例如sentry-dsn和aliyun-access-key等等，这些私密配置不能checkin到github上，所以采用了Spring的一种私密配置机制，私密数据集中配置在config/application.yml中，这个文件在gitignore中，不会被checkin到github
环境变量 spring.config.additinal-config ，读取指定配置后会继续读取默认配置


clientid/secret不能放在js单页应用里头，这样做会导致clientid/secret泄漏，黑客就可以伪造非法应用。一般js应用可以用简化(implicit)模式，只需要存clientId(没有secret)，但是这种方式仅适合安全不严格的应用。如果安全严格，又需要js单页，那么建议引入proxy代理，clientId/secret配在代理上，单页js调用后台服务通过代理转发，这种方式类似Web应用的授权码模式。

一个服务可以共用一个resttemaplate，resttemplate是线程安全的
默认resttemplate每次新建一个HttpURLConnection，也可以配连接池


没啥特别准备的，不要好高骛远，也不要妄自菲薄，去了先虚心学习，了解现状，然后找到一个自己能发挥作用的切入点，先落地下来


没啥问题，拉代码->构建->测试->打镜像->推镜像->部署到k8s，都可以在jenkins中建流水线完成，这个就是CI/CD

在github上配一个webhook，去触发你的jekins pipeline  

也可以尝试travisci/circleci这些ci/cd服务  

或者你找找jenkins插件，应该也有定期检测github的 

Copy the questions, not the answers.

中台：
企业级能力复用平台
函数->类->组件->服务->平台->中台，

中台架构，可以作为互联网组织架构和系统架构的一个参考模型(如何设计组织架构和系统架构)。
中台的核心是业务/技术能力的模块化和重用，目标是提升企业业务规模化的能力和快速响应市场需求的能力。

 你要有良好的系统抽象能力，有权支配组织甚至业务架构，可以搞搞中台

 事务拖尾日志，事务日志挖掘器

## quote

* kubernets in action
* [分布式事务论文 dzone 《Data Consistency in Microservices Architecture》](https://dzone.com/articles/data-consistency-in-microservices-architecture), by 
* [分布式事务开源解决方案-seata](https://github.com/seata/seata), by 阿里
* [ci/cd](https://github.com/fleetman-ci-cd-demo)