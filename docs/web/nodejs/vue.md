[TOC]
# VUE
# 安装
- js cdn引入<script src="https://unpkg.com/vue@2.0.1/dist/vue.js"></script>
- cdn 引入vue-resource,用于处理api调用<script src="https://cdn.jsdelivr.net/vue.resource/1.0.3/vue-resource.min.js"></script>
- el: 绑定css选择器选中元素
- v-bind: 绑定属性事件等
- v-if: 条件,可以处理dom元素是否显示
- v-for: 迭代
- data: 绑定数据模型
- method: 绑定方法
- v-model: 双向绑定
- Vue.component: 注册组件
```
// 定义名为 todo-item 的新组件
Vue.component('todo-item', {
  template: '<li>这是个待办项</li>'
})

<ol>
  <!-- 创建一个 todo-item 组件的实例 -->
  <todo-item></todo-item>
</ol>
```
- 
```
var vm = new Vue({
    el: '#vm',
    data: {
        title: 'TODO List',
        todos: []
    },
    created: function () {
        this.init();
    },
    methods: {
        init: function () {
            var that = this;
            that.$resource('/api/todos').get().then(function (resp) {
                // 调用API成功时调用json()异步返回结果:
                resp.json().then(function (result) {
                    // 更新VM的todos:
                    that.todos = result.todos;
                });
            }, function (resp) {
                // 调用API失败:
                alert('error');
            });
        },
         create: function (todo) {
            var that = this;
            that.$resource('/api/todos').save(todo).then(function (resp) {
                resp.json().then(function (result) {
                    that.todos.push(result);
                });
            }, showError);
        },
        update: function (todo, prop, e) {
            ...
        },
        remove: function (todo) {
            ...
        }
    }
});
```
```
//新增表单
var vmAdd = new Vue({
    el: '#vmAdd',
    data: {
        name: '',
        description: ''
    },
    methods: {
        submit: function () {
            vm.create(this.$data);
        }
    }
});
//html页面
<form id="vmAdd" action="#0" v-on:submit.prevent="submit">
    <p><input type="text" v-model="name"></p>
    <p><input type="text" v-model="description"></p>
    <p><button type="submit">Add</button></p>
</form>
```
```
//vm绑定的页面html
<div id="vm">
    <h3>{{ title }}</h3>
    <ol>
        <li v-for="t in todos">
            <dl>
                <dt contenteditable="true" v-on:blur="update(t, 'name', $event)">{{ t.name }}</dt>
                <dd contenteditable="true" v-on:blur="update(t, 'description', $event)">{{ t.description }}</dd>
                <dd><a href="#0" v-on:click="remove(t)">Delete</a></dd>
            </dl>
        </li>
    </ol>
</div>
```
- 表格
```
//结构
data: {
    title: 'New Sheet',
    header: [ // 对应首行 A, B, C...
        { row: 0, col: 0, text: '' },
        { row: 0, col: 1, text: 'A' },
        { row: 0, col: 2, text: 'B' },
        { row: 0, col: 3, text: 'C' },
        ...
        { row: 0, col: 10, text: 'J' }
    ],
    rows: [
        [
        	{ row: 1, col: 0, text: '1' },
        	{ row: 1, col: 1, text: '' },
        	{ row: 1, col: 2, text: '' },
            ...
        	{ row: 1, col: 10, text: '' },
        ],
        [
        	{ row: 2, col: 0, text: '2' },
        	{ row: 2, col: 1, text: '' },
        	{ row: 2, col: 2, text: '' },
            ...
        	{ row: 2, col: 10, text: '' },
        ],
        ...
        [
        	{ row: 10, col: 0, text: '10' },
        	{ row: 10, col: 1, text: '' },
        	{ row: 10, col: 2, text: '' },
            ...
        	{ row: 10, col: 10, text: '' },
        ]
    ],
    selectedRowIndex: 0, // 当前活动单元格的row
    selectedColIndex: 0 // 当前活动单元格的col
}
```
```
<!--表格html-->
<table id="sheet">
    <thead>
        <tr>
            <th v-for="cell in header" v-text="cell.text"></th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="tr in rows">
            <td v-for="cell in tr" v-text="cell.text"></td>
        </tr>
    </tbody>
</table>
```
```
<!--添加可编辑-->
<td v-for="cell in tr" v-bind:contenteditable="cell.contentEditable" v-text="cell.text"></td>
```
```
<!--添加点击和离开事件-->
<td v-for="cell in tr" v-on:click="focus(cell)" v-on:blur="change" ...></td>
```
```
//添加方法
var vm = new Vue({
    ...
    methods: {
        focus: function (cell) {
            this.selectedRowIndex = cell.row;
            this.selectedColIndex = cell.col;
        },
        change: function (e) {
            // change事件传入的e是DOM事件
            var
                rowIndex = this.selectedRowIndex,
                colIndex = this.selectedColIndex,
                text;
            if (rowIndex > 0 && colIndex > 0) {
                text = e.target.innerText; // 获取td的innerText
                this.rows[rowIndex - 1][colIndex].text = text;
            }
        }
    }
});
```
```
<!--绑定css样式-->
<td v-for="cell in tr" ... v-bind:style="{ textAlign: cell.align }"></td>
```
```
function setAlign(align) {
    var
        rowIndex = vm.selectedRowIndex,
        colIndex = vm.selectedColIndex,
        row, cell;
    if (rowIndex > 0 && colIndex > 0) {
        row = vm.rows[rowIndex - 1];
        cell = row[colIndex];
        cell.align = align;
    }
}

// 给按钮绑定事件:
$('#cmd-left').click(function () { setAlign('left'); });
$('#cmd-center').click(function () { setAlign('center'); });
$('#cmd-right').click(function () { setAlign('right'); });
```
- 