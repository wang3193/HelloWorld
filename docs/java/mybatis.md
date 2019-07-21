# Mybatis
## 使用
- 依据xml配置文件创建sqlSessionFactory
- sql配置文件,配置sql
- sql配置文件配置进全局配置文件中
- sqlSessionFactory创建sqlsession,一次sqlsession代表一次数据库会话对象
- sqlSession使用sql的唯一标识来查询
- SqlSession非现场安全
### 别名
- typeAliases标签
- typeAlias 为单个类取别名,type指定类,alias指定别名,不指定alias使用默认别名,即类名小写
- package 为整个包指定默认别名
- 在指定package时,可使用@Alias 为单个类指定别名
- 