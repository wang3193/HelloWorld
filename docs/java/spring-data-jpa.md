# Spring Data Jpa
## QueryDsl
1. [spring 官方文档](https://spring.io/projects/spring-boot#learn)  
2. [Querydsl 文档](http://books.aying.org/querydsl_zh_CN/Tutorials/Querying%20JPA.html)  
3. [SpringBoot环境下QueryDSL-JPA的入门及进阶](https://www.jianshu.com/p/69dcb1b85bbb)
## Specification
## 多表映射
### 一对多
- @OneToMany(targetEntity = childClass.class)
  - 声明关系,配置一对多,targetEntity是子表的映射类
- @JoinColumn(name=childKey, referencedColumnName=mainkey)
  - 配置关联字段, name为子表外键字段,referencedColumnName为主表对应字段
- @ManyToOne(targetEntity= mainClass.class)
  - 声明关系,targertEntity为主表对应实体
- @JoinColumn(name=cildKey, referenedColumnName=mainKey)
  - 配置关联字段,同OneToMany
- 如果配置双向关系,双方都会维护外键
- 一对多的情况下,如果一的一方要维护外键,会多发送一条update语句
- 如果一的一方要放弃维护外键权
- @OneToMany(mappedBy= childKey)
  - mappedBy里配置的是多的一方中的一方映射对象
  - 如果放弃了外键维护,则一方不能删除
  - 需要使用级联删除
- 级联属性(cascade)
  - 在操作主体上的声明上配置cascad
  - @OneToMany(mappedby='XXX', cascade= CascadeType.ALL)
### 多对多
- @ManyToMany
  - 声明关系,配置多对多,targetEntity是关联表的映射类
- @JoinTable
  - 配置中间表
    - name是中间表的名称
    - joinColumns配置当前表在中间表的外键关联
      - JoinColumn name是中间表的字段,refColName是当前表的关联字段
    - inverseJoinColumn配置关联表在中间表的关联字段
      - JoinColumn name是关联表在中间表的字段,refColName是关联表的关联字段 
- 放弃外键维护权同一对多(mappedBy=other.class)
```
@ManyToMany(targetEntity=other.class)
@JoinTable(name="mid_table", 
  joinColumns={@JoinColumn(name=midkey,referencedColumnName=thiskey)},
  inverseJoinColumn={@JoinColumn(name=otherMidKey,referencedColumnName=otherKey)}
)
```
### 延迟加载
- 关联查询不会立即查询关联对象
- 而是会在使用关联对象的时候才查询,叫延迟加载
- 如需关闭延迟加载,需要在声明关系上添加fetch=FetchType.EAGER(不推荐)