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