# Microservice Architecture with Spring Boot, Docker and Kubernets

## mono repo vs multiple repositories

http://blog.shippable.com/our-journey-to-microservices-and-a-mono-repository

## Routing and Filtering

zuul, spring cloud gateway

### Routing

Spring Cloud Netflix includes an embedded Zuul proxy, which we can enable with the @EnableZuulProxy annotation.   

To forward requests from the Gateway application, we need to tell Zuul the routes that it should watch and the services to which to forward requests to those routes. We specify routes using properties under zuul.routes. Each of our microservices can have an entry under zuul.routes.NAME, where NAME is the application name (as stored in the spring.application.name property).  
zuul.routes.books.url=http://localhost:8090
  
 
### Filtering

Zuul has four standard filter types:

- pre filters are executed before the request is routed,

- route filters can handle the actual routing of the request,

- post filters are executed after the request has been routed, and

- error filters execute if an error occurs in the course of handling the request.

 Spring Cloud Netflix picks up, as a filter, any @Bean which extends com.netflix.zuul.ZuulFilter and is available in the application context.


```java
package hello.filters.pre;

import javax.servlet.http.HttpServletRequest;
import com.netflix.zuul.context.RequestContext;
import com.netflix.zuul.ZuulFilter;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class SimpleFilter extends ZuulFilter {

  private static Logger log = LoggerFactory.getLogger(SimpleFilter.class);

  @Override
  public String filterType() {
    return "pre";
  }

  @Override
  public int filterOrder() {
    return 1;
  }

  @Override
  public boolean shouldFilter() {
    return true;
  }

  @Override
  public Object run() {
    RequestContext ctx = RequestContext.getCurrentContext();
    HttpServletRequest request = ctx.getRequest();

    log.info(String.format("%s request to %s", request.getMethod(), request.getRequestURL().toString()));

    return null;
  }

}
```

## OIDC(OpenID Connect)

## docker & docker-compose

> deploy with docker and docker-compose

## EFK

elastic search + fluentd + log parser + kafka + Kibana)

## Prometheus

metric monitoring

## GLOABL LOCK

## Reference

[spring cloud zuul](https://spring.io/guides/gs/routing-and-filtering/)
[zuul offical wiki](https://github.com/Netflix/zuul/wiki)
[oidc](https://www.cnblogs.com/linianhui/p/openid-connect-core.html)
[OpenID Connect](https://openid.net/connect/)
[fusionauth](https://fusionauth.io/docs/)