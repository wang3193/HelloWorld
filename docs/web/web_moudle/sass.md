# SASS
- SASS 是一种CSS扩展语言
## 变量
- $varible 使用$使用变量
## 嵌套
- a { h1{} h2{}} 转义后 a h1{} a h2{}
- & 父选择器标识符 a{ &:hover {} h1{}} 转义后 a:hover{} a h1{}
- 群组嵌套器 a{ h1,h2,h3{}} 转义后 a h1{} a h2{} a h3{}
## 导入SASS
- @import
- !default 默认参数 $use-width:30px !default;
## 混合器
- @mixin name 混合器定义
- @include mixinName 调用混合器
- @mixin name( param1, param2) 定义带参数混合器
- @include mixinName(param1, param2) 调用带参数混合器
- @mixin name( param1:default, param2:default) 定义带默认值参数的混合器
## 继承
- @extend selector 继承选择器样式
