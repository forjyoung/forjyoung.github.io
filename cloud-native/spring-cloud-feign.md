# Spring Cloud Feign

## 简介

Feign是Netflix开发的声明式、模板化的HTTP客户端，其灵感来自Retrofit、JAXRS-2.0以及WebSocket。Feign可帮助我们更加便捷、优雅地调用HTTP API。

## ISSUE

### 希望Feign能够支持参数请求使用POJO

*@SpringQueryMap 注解*

  I use openfeign with `org.springframework.cloud:spring-cloud-starter-openfeign:2+`
  
  ```
  @Autowired(required = false)
  private ConversionService feignConversionService;

  @Bean
  public Contract feignContract() {
    return new SpringMvcPojoObjectQueryContract(this.parameterProcessors, feignConversionService);
  }
  ```
  
  ```
  import feign.MethodMetadata;
import java.lang.annotation.Annotation;
import java.util.List;
import org.apache.commons.lang3.StringUtils;
import org.springframework.cloud.openfeign.AnnotatedParameterProcessor;
import org.springframework.cloud.openfeign.support.SpringMvcContract;
import org.springframework.core.convert.ConversionService;
import org.springframework.web.bind.annotation.RequestBody;

/**
 * Created by jiangyu on 2019-01-10 22:52.
 */
public class SpringMvcPojoObjectQueryContract extends SpringMvcContract {

  public SpringMvcPojoObjectQueryContract(List<AnnotatedParameterProcessor> annotatedParameterProcessors,
      ConversionService conversionService) {
    super(annotatedParameterProcessors, conversionService);
  }

  @Override
  protected boolean processAnnotationsOnParameter(MethodMetadata data, Annotation[] annotations, int paramIndex) {
    boolean httpAnnotation = super.processAnnotationsOnParameter(data, annotations, paramIndex);
    //在springMvc中如果是Get请求且参数中是对象 没有声明为@RequestBody 则默认为Param
    if (!httpAnnotation && StringUtils.equalsIgnoreCase(data.template().method(), "GET")) {
      for (Annotation parameterAnnotation : annotations) {
        if (!(parameterAnnotation instanceof RequestBody)) {
          return false;
        }
      }
      data.queryMapIndex(paramIndex);
      return true;
    }
    return httpAnnotation;
  }
}

  ```