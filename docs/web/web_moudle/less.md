<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [LESS](#less)
  - [安装](#%E5%AE%89%E8%A3%85)
  - [编译](#%E7%BC%96%E8%AF%91)
  - [变量](#%E5%8F%98%E9%87%8F)
  - [mixins 混排](#mixins-%E6%B7%B7%E6%8E%92)
  - [嵌套](#%E5%B5%8C%E5%A5%97)
  - [继承](#%E7%BB%A7%E6%89%BF)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

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

