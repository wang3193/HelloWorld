# rocketmq
## 简介
- 不遵循任何规范
### 术语简介
- producer 消息生产者
- consumer 消息消费者
- push consumer consumer的一种,通常向consumer对象注册一个listen接口,一旦收到消息,立刻回调listener接口方法
- pull consumer 使用consumer的拉消息方法从broker拉消息,主动权由应用控制
- producer group producer的集合名称,这类producer通常发送一类消息,且发送逻辑一致
- consumer group consumer的集合名称,这类consumer通常消费一类消息,且消费逻辑一致
- broker 消息中转角色,负责存储消息,转发消息
