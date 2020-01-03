## Gateway

### The basic function

![08cdd28824c3f7f8785055a577c8ceae.png](evernotecid://EE7FFED1-EFD2-4384-B560-E4FC2DE751E5/appyinxiangcom/7220699/ENResource/p1449)

### Major open source gateways


|  | 支持公司 | 实现语言 | 描述 |
| --- | --- | --- | --- |
| Nginx(2004) | Nginx Inc | C/Lua | |
| Kong(2014) | Kong Inc | OpenResty/Lua | |
| Zuul(2012) | Netflix/Pivotal | Java |  Zuul1/Zuul2/Spring Cloud Netflix Zuul（Pivotal）  |
| Spring Cloud Gateway(2016) | Pivotal | Java | |
| Envoy(2016) | Lyft | C++ | 方便 ServiceMesh 集成 |
| Traefik(2015) | Containous | Golang |    |

### Purpose

1. payment 服务鉴权（现暴露 payment 服务需要经由 op 或者 onboarding 转发，需要额外开发）
1. 服务内部调用无法鉴权

![f049fca147c5a5f25652defacc397e64.jpeg](evernotecid://EE7FFED1-EFD2-4384-B560-E4FC2DE751E5/appyinxiangcom/7220699/ENResource/p1451)

![0b55c335589a941db368551c56c109a9.png](evernotecid://EE7FFED1-EFD2-4384-B560-E4FC2DE751E5/appyinxiangcom/7220699/ENResource/p1450)


### Code Review

1. dependencies
2. properties
3. @Enable
4. filter
    - PRE：这种过滤器在请求到达Origin Server之前调用。比如身份验证，在集群中选择请求的Origin Server，记log等。
    - ROUTING：在这种过滤器中把用户请求发送给Origin Server。发送给Origin Server的用户请求在这类过滤器中build。并使用Apache HttpClient或者Netfilx Ribbon发送给Origin Server。使用 zuul 内部 filter 不需要自定义。
    - POST：这种过滤器在用户请求从Origin Server返回以后执行。比如在返回的response上面加response header，做各种统计等。并在该过滤器中把response返回给客户。
    - ERROR：在其他阶段发生错误时执行该过滤器。
    - 客户定制：比如我们可以定制一种STATIC类型的过滤器，用来模拟生成返回给客户的response。


#### filter

1. routing
    1. http client
    2. 他们是通过一个RequestContext的静态类来进行数据传递的。RequestContext类中有ThreadLocal变量来记录每个Request所需要传递的数据。
![b0fc9653ee70d34635c536e6a4bd9f64.png](evernotecid://EE7FFED1-EFD2-4384-B560-E4FC2DE751E5/appyinxiangcom/7220699/ENResource/p1453)

### Issue

1.[cookie](https://blog.csdn.net/lindan1984/article/details/79308396)

### TODO

1. 不对外暴露的接口
    1. AuthFilter 添加 UNEXPOSED_URIS,不做转发
2. 长连接/短链接选择
    - 我们使用 apache http client 做为 zuul 的 http 工具（内置 routing file   org.springframework.cloud.netflix.zuul.filters.route.SimpleHostRoutingFilter)，http 属性可通过
        - httpClient.maxTotal=200
        -httpClient.defaultMaxPerRoute=50
        - httpClient.connectTimeout=1000
        - httpClient.connectionRequestTimeout=500
        - httpClient.socketTimeout=10000
       
   等 http client 配置调整。默认使用短链接
    - zuul 默认配置参考 org.springframework.cloud.netflix.zuul.filters.ZuulProperties.Host
    - WEB 应用建议采用短链接

3. 限流熔断

jemeter



### Reference

- [Zuul](https://github.com/Netflix/zuul)
- [Nginx](https://www.nginx.com)
- [Kong](https://konghq.com/kong)
- [Traefik](https://traefik.io)
- [Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway)
- [Spring Cloud Netflix](https://cloud.spring.io/spring-cloud-netflix/multi/multi__router_and_filter_zuul.html)
- [zuul 文章](https://www.jianshu.com/p/e0434a421c03)