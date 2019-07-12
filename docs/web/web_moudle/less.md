# LESS
## 安装
- npm install -g less
## 编译
- lessc style.less style.css
## 变量
- @nice-blue: #5B83AD 使用@name定义使用变量
## mixins 混排
- .class1{} h1 {color:red;.class} 之前的样式可以直接混排进后面的css中
## 嵌套 
- h1{ a{} .class{}}
## 继承
- :extend(.class) 

