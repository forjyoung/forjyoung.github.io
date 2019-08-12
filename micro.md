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


## talk 

本课程案例相对简单，数据库表不多，不涉及分布式事务。关于分布式事务一致性，推荐参考dzone上的文章《Data Consistency in Microservices Architecture》(https://dzone.com/articles/data-consistency-in-microservices-architecture)。另外阿里开源的分布式事务组件seata(https://github.com/seata/seata)社区很热，可以参考。

stackflow上有个贴，讲java时间类的差异和用法，配图，可以参考：       
https://stackoverflow.com/questions/32437550/whats-the-difference-between-instant-and-localdatetime

为何不用Spring 事件监听模式，结合spring的异步去使用，而是自己去维护调用上下文？

你好，Idea里头报错是因为没有启用lombok，请启用一下，方法可以在网上搜一下。我的微信号:bulldog2015，加微信说明来自极客时间。

ReverseProxyFilter通过FilterRegistrationBean注册到Spring容器环境中，Spring会自动将这个Filter注册到Web容器中。参考faraday项目源码中的config/FaradayConfiguration这个Bean配置文件。

在 jwt 的公私钥模式中，私钥存在 Auth 服务器上，不能存客户端，公钥一般也不存客户端(存网关或者后台服务器上)，即使存客户端也没有大关系，公钥本来就是公开的，只不过能够解签jwt，不能篡改数据。

## quote

* [分布式事务论文 dzone 《Data Consistency in Microservices Architecture》](https://dzone.com/articles/data-consistency-in-microservices-architecture), by 
* [分布式事务开源解决方案-seata](https://github.com/seata/seata), by 阿里