## CSS variable

css变量减少样式重复定义，比如同一个颜色值要在多个地方重复使用，以前通过less和sass预处理做到，现在css变量也可以做到，方便维护，提高可读性。

```js
:root{
  --bgcolor: blue;
  --color: red;
}
p {
  color: var(--color);
}
div {
  backgroung-color: var(--bgcolor);
  color: var(--color)
}
```

## html 的默认 display 属性是多少

```css
display:block
```

## 元素垂直水平居中

flex布局

## display: inline 的元素设置 margin 和 padding 会生效吗

inline 元素的 margin 与 padding 左右生效，上下生效，**准确说在上下方向不会使其它元素受到挤压，仿佛不生效**

## 实现表格单双行条纹样式

通过 `css3` 中伪类 `:nth-child` 来实现。其中 `:nth-child(an+b)` 匹配下标 `{ an + b; n = 0, 1, 2, ...}` 且结果为整数的子元素

- `nth-child(2n)`/`nth-child(even)`: 双行样式
- `nth-child(2n+1)`/`nth-child(odd)`: 单行样式

## css specificity

`css specificity` 即 css 中关于选择器的权重，以下三种类型的选择器依次下降

1. `id` 选择器，如 `#app`
2. `class`、`attribute` 与 `pseudo-classes` 选择器，如 `.header`、`[type="radio"]` 与 `:hover`
3. `type` 标签选择器和伪元素选择器，如 `h1`、`p` 和 `::before`

其中通配符选择器 `*`，组合选择器 `+ ~ >`，否定伪类选择器 `:not()` 对优先级无影响

##  '+' 与 '~' 选择器

- `+` 选择器匹配紧邻的兄弟元素
- `~` 选择器匹配随后的所有兄弟元素

## css 动画与 js 动画

### CSS3的动画：

1.在性能上会稍微好一些，浏览器会对CSS3的动画做一些优化（比如专门新建一个图层用来跑动画）

2.代码相对简单 　

3.在动画控制上不够灵活 　

4.兼容性不好 　　

5.部分动画功能无法实现（如滚动动画，视差滚动等）

###  JavaScript的动画：

 正好弥补了css缺点，控制能力很强，可以单帧的控制、变换，同时写得好完全可以兼容IE6，并且功能强大。 总结： 对于一些复杂控制的动画，使用javascript会比较好。而在实现一些小的交互动效的时候，可以多考虑CSS

## position: sticky

`position: sticky` 可理解为 `relative` 与 `fixed` 的结合体

##  伪类与伪元素

1. 伪类使用单冒号，而伪元素使用双冒号。如 `:hover` 是伪类，`::before` 是伪元素
2. 伪元素会在文档流生成一个新的元素，并且可以使用 `content` 属性设置内容

## CSS 隐藏页面中某个元素

```css
display:none;
opacity:0;
visibility:hidden;
content-visibility;
绝对定位于当前页面的不可见位置
字体大小设为0
```

## HTML 标签有哪些行内元素

- a

- img

- picture

- span

- input

- textarea

- select

- label

  ## BFC

1.符合一些特性的HTML标签，不脱离文档流，属于普通流。隔离的独立容器，容器内部的元素不会影响其他元素

触发条件 1.浮动元素。float除了none以外的值

2.绝对定位元素 absolute fixed

3.

```css
display:inline-block;table-cell ;table-caption;flex
```

4.overflow除了visible以外的值

5.body

## rem、em、vw、vh

他们同属于 CSS Data Type 中的 `[length]`

- `rem`: 根据根元素(即 `html`)的 `font-size`
- `em`: 根据**自身元素**的 `font-size`
- `vw`: viewport width
- `vh`: viewport height

## css的加载会阻塞DOM树解析和渲染吗

css的加载不会阻止DOM树的解析
css的加载会阻止DOM树的渲染，因为css的下载完成后解析成CSSDOM与DOM生成渲染树后，页面才会渲染，绘制出来

## 盒模型
- W3C的标准盒模型
在标准的盒子模型中，width指content部分的宽度
- IE的盒模型
在IE盒子模型中，width表示content+padding+border这三个部分的宽度

## 清除浮动方法

- 使用带clear属性的空元素
```css
.clear {
  clear: both;
  }
```

- 使用CSS的overflow属性
```css
overflow: hidden;
```
- 给浮动的元素的容器添加浮动
- 使用邻接元素处理
- 使用CSS的:after伪元素

## 文本溢出
- 单行
```css
.hidden {
  overflow: hidden;（文字长度超出限定宽度，则隐藏超出的内容）
  white-space: nowrap;（设置文字在一行显示，不能换行）
  text-overflow: ellipsis;（规定当文本溢出时，显示省略符号来代表被修剪的文本）
}
```

- 多行
```css
.hidden {
  -webkit-line-clamp: 2;（用来限制在一个块元素显示的文本的行数，2 表示最多显示 2 行。为了实现该效果，它需要组合其他的 WebKit 属性）
  display: -webkit-box;（和 1 结合使用，将对象作为弹性伸缩盒子模型显示 ）
  -webkit-box-orient: vertical;（和 1 结合使用 ，设置或检索伸缩盒对象的子元素的排列方式 ）
  overflow: hidden;（文本溢出限定的宽度就隐藏内容）
  text-overflow: ellipsis;（多行文本的情况下，用省略号 “…” 隐藏溢出范围的文本)
}
```