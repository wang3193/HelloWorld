# 事件监听器
- document.addEventListener('事件',function(){});
- async 异步加载js,只对外部js有效 
- defer 按顺序加载js
# 变量
## 变量类型
- Number
- String
- Boolean
- Array
- Object
## let和var的区别
- var可以提升,不提前申明,可重复申明,let更严格
- 现代浏览器建议使用let
## const 常量
## typeof 查看变量类型
## String
- slice(0,3) 截取角标0开始3个字符
- toLowerCase() 转小写
- toUpperCase() 转大写
- replace('a','b') b替换a
## Array
- push 插入最后
- pop 删除最后
- unshift 插入首位
- shift 移除首位

# 事件
- e 事件函数的参数中放入表示事件对象 eg: element.addEventListen('click', function(e){})
- e.target 事件对象e的元素引用
- e.preventDefault() 阻止事件默认行为,例如表单提交
- e.stopPropagation() 阻止事件冒泡行为
## 事件捕获
## 事件冒泡
## 事件委托

# 对象
- var obj = {}
- var myNotification = new Notification('Hello!') 使用构造器实例化对象
- var obj = new Object() 使用Object对象构建
- var obj2 = Object.create(obj1) 基于obj1对象创建obj2对象
- call() 调用一个在别处定义的函数
```
//javascript继承
function Person(){
    let age;
    let name;
}
function Teacher(){
    Person.call(this);
    let suject;
}
//创建父类的原型对象
Teacher.prototype = Object.create(Person.prototype);
//构造函数指向的事Person,需要手动修正
Teacher.prototype.constructor = Teacher;
```
## 原型链
- prototype 
- function doSomething(){}
doSomething.prototype.foo = "bar"; 对象的原型上添加foo属性
- prototype 上的属性和方法可以被之后的对象继承
- constructor 构造器属性
- 一般在构造器中定义属性,在prototype上定义方法
## 原型链的参考链

## JSON
- parse() 字符串转json
- stringfy() json转字符串
- 

# 请求
```
//http 请求
var request = new XMLHttpRequest();
request.open('GET', requestURL);
request.responseType = 'json';
request.send();
request.onload = function() {}
```
- fetch(url).then(res=>response.json()).then(data=>{}).catch(error=>{})
## 并发
- Promise 
- Promise.all([a,b,c]).then() 将多个异步请求合并处理,当有一个失败即失败,按传入数组顺序返回成功信息.
- Promise.race([a,b,c]).then() 返回最先返回的结果值
- new Promise(function(){})
- async function(){} 使用async修饰function,会使function返回一个Promise对象
- await 只能用于promise对象中,用于处理then链,可以接收多参数.

# 定时器
- setTimeout(callback, millions) millions之后执行callback一次
- setTimeout(funName, millions, param) 执行方法名为funName,参数为param的方法
- setInterval(callback, millions) 每隔millions毫秒执行callback一次
- requestAnimationFrame(callback) 每隔一帧执行callback一次
- clearTimeout(name) 清除定时器 
- clearInterval(name) 清除定时器
- cancelAnimationFrame(name) 清除定时器

# Web API
## window 浏览器对象
## navigator 用户状态对象
## document 页面对象
- Document.querySelector()
- Document.querySelectorAll()
- Document.createElement()
- Document.appendChild()
- Document.createTextNode()
- parentNode.removeChild(node)
- node.parentNode.removeChild(node)
- element.style 操作内联样式
## 客户端存储
### sessionStorage 关闭浏览器时会丢失
### localStorage 本地数据
- localStorage.setItem('key', value) 存储数据
- localStorage.getItem('key') 获取数据
- localStorage.removeItem('key') 删除数据
### indexedDB
- var req = window.indexedDB.open(dbname, version) 打开数据库
- req.onerror = function(event){}
- req.onsuccess = function(event){ db = req.result;}
- req.onupgradeneeded = function(event){req.target.result;} version大于实际版本,数据库升级
- db.createObjectStore(tableName, {keyPath: 'id'}) 创建表,主键为id
- db.transaction([tableName], 'readwrite') 创建事务,readonly/readwrite, 在操作数据库之前需先定义事务
- .add() 添加数据
- .get(keypath) 查询数据
- .put() 更新数据
- .delet(keypath) 删除数据
- createIndex(indexname, calname, {option}) 创建索引,只有创建索引才可使用索引字段查询,否者只能用主键查询
- db.objectStore(tableName).index(indexName).get() 使用索引查询
