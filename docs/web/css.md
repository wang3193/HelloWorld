# 选择器
## 组合器
- A>B B是A的直接子节点
- A+B B是A的下个兄弟节点
- A~B B是A的兄弟节点
## 属性选择器
- [abbr] 包含abbr属性的元素
- [attr="a"] 属性attr等于a的元素
- [attr~="a"] 属性attr里包含a的元素
- [attr|="a"] 属性attr为a或a-开头的元素
- [attr$="a"] 属性attr以a开头的元素
- [attr*="a"] 属性attr包含a的元素

## 伪类
- :checked 选中的元素,用于input
- :disabled 禁用的元素
- :empty 没有子元素的元素
- :enabled 启用的元素
- :first-of-type 第一个目标元素 eg: p:first-of-type 第一个为p的元素
- :in-range 在该元素指定样式范围内的值 eg: input:in-range <input type="number" min="5" max="10" value="7" />
- :invalid 所有无效的元素
- :last-child 最后一个子元素
- :last-of-type 最后一个目标元素
- :not 除目标之外的元素 eg: not(p) p以外的元素
- :nth-child(n) 第n个子元素
- :nth-last-child 倒数第n个子元素
- :nth-of-type(n) 第n个目标元素
- :nth-last-of-type(n) 倒数第n个目标元素
- :only-child 只有一个子元素的元素 eg: p:only-child 只有一个子元素的p元素
- :out-of-range 指定范围外的元素
- :read-only 只读的元素
- :read-write 非只读的元素
- :required 必填的元素
- :root 文档的根元素
- :valid 有效的元素
- :link 未访问连接
- :visited 已访问连接
- :hover 鼠标悬停
- :active 正在活动连接
- :focus 具有焦点的元素
- :first-letter 元素的第一个字母
- :first-line 元素的第一行
- :first-child 第一个子元素
- :before 元素前
- :after 元素后 

# 数值
- em 父类字体大小,默认是16px

# 网格系统
- skeleton
- Foundation
- bootstrap

# 字体
- font-family 字体
- font-size 字号
- color 字色
- font-weight:bolder 粗体
- font-style:italic 斜体
- font-decoration:overline 上划线 line-through 删除线 underline 下划线

# 段落
- text-indent:2em 段落缩进2字符
- line-height:2em 行间距
- word-spacing:2em 单词间距
- letter-spacing:1em 文字间距
- text-align:left 文字左对齐 center 居中 right 右对齐
## 多列
- column-count: 3 分成3列
- column-gap:30px 中间间隙30px
- column-rule-style: solid 中间分隔样式
- column-rule-width: 1px 间隔边框厚度
- column-rule-color: blue 间隔颜色

# 布局 
## flex
- display:flex 
- justify-content:flex-start 横向左对齐 center 居中 flex-end 右边对齐 stretch 两端对齐 space-round 均分 space-between  space-evenly
- align-items:flex-start 纵向上对齐 center 居中 flex-end 下对齐 stretch 两端对齐 space-round 均分 space-between  space-evenly
- align-self: 单独操作对齐,同align-item.
- flex-direction:row 水平排列 row-reverse 水平反序 column 垂直 column-reverse 垂直反序
- flex-grow:1 弹性因子,没有指定时默认都为1,占有指定比例大小
- flex-basis: 定义flex初始大小
- flex-shrink: 元素收缩规则
- flex-wrap: wrap 元素超出页面自动换行
  
## 网格(grid)
- display:grid
- grid-template-columns: repeat(12, 1fr); 分成12份栅格
- grid-gap: 20px; 栅格的空隙
- grid-column: auto/span 2 2格大小
- grid-column: 2/8 第2到第8格
- grid-row: 3/5 3到5行
  
## float
- float:left 左浮动 right 右浮动

## visibility
- visibility:visible|hidden 元素可见|不可见但影响布局

## absolute
- position:absolute 绝对布局
- top: 据上方 left: 左 buttom: 下 right: 右

## relative
- position:relative 相对布局
- 父元素使用相对布局,子元素使用绝对布局

## fixed
- postion:fixed 固定屏幕
  
## input
- resize:both 可以让用户调整框型大小
 
# 响应式设计
- <meta name="viewport" content="width=device-width, initial-scale=1.0"> 添加viewpoint设置
- @media only screen and (min-width: 600px) 屏幕宽度大于600px
- @media only screen and (max-width: 768px) 屏幕宽度小于768px
- orientation：portrait | landscape 竖屏|横屏
- @media only screen and (orientation: portrait) 判断是否是竖屏
- maxwidth: 600px 设置图片的最大宽度为600px
-  
 
 
