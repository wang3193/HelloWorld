# Python
## 基础语法
```
#下面的注解指定了脚本解释器地址,为本机python3所在位置,例如/usr/local/Cellar/python/3.7.4/bin为我本机python3所在地址
#!/usr/bin/python3
'''
使用3个'或者"来做多行注释
'''
#使用\结尾可以将单行分为多行
total = 'python'+\
        'Hello'
#使用;可以将多行语句放到一行中
a = 'a'; b = 'b'; c = a+b
"""
条件判断语句
"""
if expression:
    suite
else if expression:
    suite
else:
    suite
```
## 变量类型
- Numbers 数字型
- String 字符型
- List 列表
- Tuple 元组
  - 元祖和列表的差别在与元祖内的元素不可变化
- Dictionary 字典
```
#int/long型
a = 1
#float型
a = 1.5
#complex复数型
a = 3.14j
#字符串
s = "str"
#List
l = [1,"stt"]
#Tuple
t = (2,"s")
#字典
d = {"b":"v1",'c':123}
d['a']="value"
```
## 运算符
### 运算符
- +,-,*,/,%,**,//
- 加,减,乘,除,取模(取余数),乘方(a**b,a的b次方),取整除,向下取整
```
a = 13
b = 2
print(a + b) 
print(a - b) 
print(a * b) 
print(a / b) 
print(a % b) 
print(a ** b) 
print(a // b) 
```
### 比较符
- ==,!=,<>,>,<,>=,<=
- 等于,不等于,不等于,大于,小于,大于等于,小于等于
### 赋值运算符
- =,+=,-=,*=,/=,%=,**=,//=
```
#等价于a=a+b
a+=b
#等价于a=a//b
a-=b
```
### 位运算符
- &,|,^,~,<<,>>
- 按位与,按位或,按位异或,按位取反,左移运算符,右移运算符
### 逻辑运算符
- and,or,not
- 且,或,非
### 成员运算符
- in, not in
- 在序列中,不在序列中
```
a = 1
b = [1,2,3,4,5]
print(a in b)
print(a not in b)
```
### 身份运算符
- is, is not
- 判断是否引用自一个对象,即判断内存地址是否一致,a is b 类似于id(a)== id(b)
- id()函数用于获取对象的内存地址

## 条件语句
- if,else if,else
```
if cond:
    do
else if cond:
    do
else:
    do
```
## 循环语句
- while循环
```
while cond:
    do
```
- for循环
```
for var in arr:
    print(var)
for index in range(len(arr)):
    print(arr[index])
```
- 嵌套循环
```
for var1 in arr1:
    for var2 in arr2:
        print(var2)
    print(var1)
```
- break 退出最深处循环
- continue 退出当前循环剩余语句,开始下次循环
- pass 空语句,用于占位
## Number
- 使用del可以删除Number引用
- math模块定义很多对浮点数的运算
```
import math
dir(math)
```
- cmath模块定义对复数的运算
```
import cmath
dir(cmath)
```
## 函数
- 使用def定义函数
- 函数内容以:开始,保持缩进
- 第一行语句可以选择性使用字符串作为描述
- return 结束函数,可选择性的返回值,不带表达式的return相当于返回None

## 模块
- import module
- from file import fun
- 模块搜索路径1.当前路径2.搜索shell变量PYTHONPATH的每个目录3.查看默认目录,一般为/usr/local/lib/python
- 设置PYTHONPATH变量:set PYTHONPATH=/usr/local/lib/python
## 文件IO
- raw_input 按行读取屏幕输入
- input 读取屏幕输入,可输入python函数,读取到函数执行结果
- open(file) 打开文件
- file.close 关闭文件
- file.write(str) 写入文件
- file.read() 读取文件
## 面向对象
- class 类:描述相同属性方法的集合,对象是类的实例
- 类变量 在整个实例化的对象中是公用的,类似java的static变量
```
class Employee:
    "此处为类描述"
    empCount = 0 #empCount为类变量  #empCount为类变量,可使用Employee.empCount来访问
    def __init__(self, name, salary):  #__init__为类的构造函数或初始化方法,创建类的时候会调用该方法,self代表类的实例,在定义类的方法中必须有,调用时不必传入
        self.name = name  #self.name, self.salary 为实例变量
        self.salary = salary
        Employee.empCount += 1
    
    def displayCount(self):
        print "Total Employee %d" % Employee.empCount
#实例化对象并调用对象方法
employee = Employee("name",200)
emp.displayCount()
```
### 内置类属性
- __dict__ 类的属性
- __doc__ 类的文档字符串
- __name__ 类名
- __module__ 类定义所在模块
- __bases__ 类的所有父类构成元素