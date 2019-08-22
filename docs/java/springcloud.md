# SpringCloud
## 微服务
- 将单一应用程序划分为一组小程序
- 每个小程序都是一个进程
- 进程间使用轻量级通信
- 
## Eureka
- 服务注册与发现
- 优点:
  - spring cloud 背书
  - AP模型,最终一致性
  - 开箱即用
- 缺点:
  - 内存限制,客户端上报完整注册信息,服务端内存浪费
  - 单一调度更新,轮询调度,增加服务器压力
  - 集群伸缩性限制,广播式复制模型
### Eureka service
- spring-cloud-starter-eureka-service
- eureka config
```
eureka:
    instance:
        hostname: localhost # eureka服务端实例名称
    client:
        register-with-eureka: false # 表示不向eureka注册自身
        fetch-registry: false   # 表示自身就是注册中心,不需要检索服务
        service-url:
            defaultZone: http://${eureka.instance.hostname}
```
- @EnableEurekaServer
### server provider
- spring-cloud-starter-eureka
- spring-cloud-starter-config
- config
```
spring:
    application:
        name: appName  # 暴露给eruka的服务名
eureka:
    instance: 
        instance-id: serverName  # eureka服务列表里的节点名称
        prefer-ip-address: true  # 显示节点的ip地址
    clent:
       service-url:
            defaultZone: http://eurekaUrl/eureka 
```
- @EnableEurekaClient
- 节点info页信息
  - spring-boot-starter-actutor
  - 父工程添加配置文件路径,以及解析关键字
  - config
```
info:
    app.name: appname
    company.name: companyname
    build.artifactId: $project.artifactId$
```
### server customer
- 使用restTemplate做http调用
- DiscoveryClient导入
- @EnableDiscoveryClient
- 

## Ribbon
- 负载均衡
  - 集中式 硬件
  - 进程类 软件   
- customer里添加
  - spring-cloud-starter-ribbon
  - spring-cloud-starter-eureka
  - spring-cloud-starter-config
  - config
  ```
  eureka:
    client:
        register-with-eureka: false 不将自身注册进eureka
        service-url:
            defaultZone: eurekaUrl
  ```
  - RestTemplate Bean类上加@LoadBalance注解,开启负载均衡
  - @EnableEurekaClient
  - url-prefix设置applicationname
- 添加IRule的bean return不同的rule类来选择负载均衡的算法
- 自定义算法
  - @RibbonClient(name=applicationName,configuration=rule.class)
  - 
## Feign
- 负载均衡
- 声明式webservice客户端
- api项目
  - spring-cloud-starter-feign
  - 创建service类
  - service类创建@FeignClient(value=applicationName)
  - service类添加provider项目中的调用方法
- customer项目
  - pom: spring-cloud-starter-feign
  - @EnableFeignClients(basePackage='')
  - @CompanionScan(package)
  - @Auto api的service类
## Hystrix
- 断路器
- 处理分布式系统延迟和容错的开源库,保证在一个依赖出现问题的情况下,不会导致整体的服务失败,避免级联故障,提高分布式系统的弹性
- provider (服务熔断)
  - spring-cloud-starter-hystrix
  - 在controller上添加@HystrixCommand(fallbackMethod=method)
- api (服务降级)
  - @FeignClient(value=applicationName,fallbackFactory=fallbackclass)
  - 创建fallbackclass 实现 FallbackFactory<serviceclass>
  - override create方法,添加熔断方法
- customer (服务降级)
  - config添加
  ```
  feign:
    hystrix:
        enabled: true
  ```
- hystrix dashboard
  - 准实时的调用监控
  - customer 项目
  - spring-cloud-starter-hystrix
  - spring-cloud-starter-hystrix-dashboard
  - @EnableHystrixDashboard
## Zuul 
- 路由网关
- 代理,路由,过滤
- 路由将外部请求转发到具体的微服务实例上
- 过滤对请求的处理过程进行干预
- zuul会注册进eureka
- 创建getway项目
- spring-cloud-starter-eureka
- spring-cloud-starter-zuul
- @EnableZuulProxy
- 配置zuul地址映射
```
zuul:
    prefix: prefix #设置统一前缀
    ignored-services: applicationName/"*" #隐藏单个/所有真实服务名
    routes:
        mydept.serviceId: applicationName
        mydept.path: /mydept/**
```
## springcloud-config
- 分布式配置中心
- 集中的,动态的配置中心
- git 创建config仓库
- config 
```
spring:
    profiles:
        active:
        - dev
---
spring:
    profiles: dev
    application:
        name: applicationName
___
spring:
    profiles: test
    application:
        name: appicationName
#必须使用utf-8编码
```
- service 端:
  - spring-cloud-starter-config-server
  - config
  ```
  spring:
    application:
        name: applicationName
    cloud:
        config:
            server:
                git:
                    uri: gitUri
  ```
  - @EnableConfigServer
- customer 端
- spring-cloud-starter-config
- 添加bootstrap.yml
- bootstrap.yml:
```
spring:
    cloud:
        config:
            name: applicationName
            profile: dev
            label: master
            uri: config-server-url
```

