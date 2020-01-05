# springboot
## 微服务
- 一个应用应该是一组小型服务,服务间通过http进行互联
- 每一个功能元素都是一个可独立替换和独立升级的功能单元
## 场景启动器
- spring-boot-starter-parent
  - spring-boot-dependencies
  - 定义了spring-boot的jar包依赖管理,jar包版本仲裁中心
- spring-boot-starter-*
  - 导入开发需要的依赖 
## 主入口
- @SpringBootApplication  主入口注解
  - @SpringBootConfiguration springboot配置类注解
    - @Configuration 配置类注解
  - @EnableAutoConfiguration 开启自动配置功能
    - @AutoConfigurationPackage 自动配置包
    - 将主配置类所在包下所有子包中的组件均扫描进spring
    - @import 导入组件
      - EnableAutoConfigurationImportSelector 导入组件选择器
      - 会给容器导入自动配置类(*AutoConfiguration)
## resource
- static 保存所有的静态资源
- templates 保存所有的模板页面
- application.properties springboot配置文件
## 热部署
- pom
  - springloaded
  - spring-boot-devtools
## 参考资料
- [springboot一键部署到远程docker](https://mp.weixin.qq.com/s/vSCQLvQBYMYoPhdlO2v3XA)
- [Nginx极简入门教程](https://mp.weixin.qq.com/s/ZN07_3ImmyRU0NQaqzcazQ)
- [nginx部署前后端分离项目,处理跨域问题](https://mp.weixin.qq.com/s/okKU5VmBxwyht09TGTNdBQ)
## servlet3.0
- servlet3.0采用注解方式注册listener,servlet,filter
  - @WebServlet
  - @WebFilter
  - @WebListener
- 使用SPI机制
  - 在MATA-INF下创建service/javax.servlet.ServletContainerInitializer
  - 在ServletContainerInitializer实现类中在servletContext中添加addServlet(),addListener(),addFilter()
  - @HandlerType(interface.class) 会在ServletContainerInitializer实现类中
## 扩展springMvc
- spring 1.X extends WebMvcConfigAdapteron
- spring 2.x implements WebMvcConfigurer 
  - extends WebMvcConfigureSupport 会导致springboot的springmvc默认配置失效
- 如果使用@EnableWebMvc会使springboot的springmvc的默认配置失效
## springBoot 错误处理
- 在静态文件夹或者模板文件夹下放error文件夹,内部放错误状态码.html文件
- 自定义异常处理
  - @ControllerAdvice 定义异常处理类, @ExceptionHandler(自定义异常.class)定义异常处理方法.
## 侵入式servletr容器
- 使用embeddedServletCustomizer来定制自定义容器属性
- 配置文件中使用server.配置容器对应属性
- 注册3大组件
  - ServletRegistrationBean
  - FilterRegistrationBean
  - ServletListenerRegistrationBean

## 使用其他servlet容器
- 默认使用tomcat
- jetty适合长连接应用
- Undertow不支持jsp,但是高并发性能较好
- 切换方式,在spring-boot-web中exclude spring-boot-starter-tomcat,引入要使用的servlet的pom文件
- 使用外部tomcat
  - 内置tomcat不能处理jsp
  - 将打包方式改成war包
  - 将spring-boot-starter-tomcat改为provided
  - 编写一个springBootServletInitializer的子类,并调用configure方法
  ```
    public class ServletInitializer extends SpringBootServletInitializer{

       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application){
         //传入springboot主程序
         return application.sources(springbootmain.class);
       }
    }
  ```

## 项目开发
### 创建自定义注解校验规则
1. 创建注解@interface
2. 实现ConstraintValidator<>

### 同意异常处理
1. 使用@RestControllerAdvice类
2. 使用@ExceptionHandler(MyException.class)来处理对应的异常
```
@RestControllerAdvice
public class GlobalException {
  @ExceptionHandler(MyException.class)
  public MyResponse handlerError(MyException e){
      return new MyResponse();
  }
  //使用throwable来处理捕获不到的异常类
  @ExceptionHandler(Throwable.class)
  public MyResponse handlerError(Throwable e){

  }
}

```
- springmvc中处理同意异常,实现ErrorController.

### 设置异步处理复制线程上下文
1. 在config java类中创建Excutor bean对象
2. bean对象中设置ContextCopying类(实现TaskDecorator)
```
public class ContextCopying implements TaskDecorator {
  @Override
  public Runnable decoratate(Runnable runnable) {
    RequestAttributes context = RequestContextHolder.currentRequestAttribute();
    return () -> {
      try {
        RequestContextHolder.setReqyestAttributes(context);
        runnable.ru();
      }finally {
        RequestContextHolder.requestAttributes();
      }
    }
  }
}

public class Appconfig {
  @Bean(name="asyncExecutor")
  public Executor asyncExecutor () {
    ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
    executor.setTaskDecorator(new ContextCopying());
    ...s
    return executor;
  }
}

```

### JWT实现单点登录
- jwt定义 json web token
- jwt构成 header payload signature
- 3部分均使用base64编码


