## promise.all 的实现

```js
Promise.all = function(arr) {
  return new Promise((resolve, reject) => {
    const ans = [];
    let index = 0;
    for (let i = 0; i < arr.length; i++) {
      arr[i]
        .then(res => {
          ans[i] = res;
          index++;
          if (index === arr.length) {
            resolve(ans);
          }
        })
        .catch(err => reject(err));
    }
  });
};
```

扩展 Promise.race

```js
Promise.race = function(arr) {
  return new Promise((resolve, reject) => {
    arr.forEach(p => {
      Promise.resolve(p).then(
        val => resolve(val),
        err => reject(err),
      );
    });
  });
};
```

## 字符串千分符

```js
function format(num) {
  num = num + ''; //数字转字符串
  var str = ''; //字符串累加
  for (var i = num.length - 1, j = 1; i >= 0; i--, j++) {
    if (j % 3 == 0 && i != 0) {
      //每隔三位加逗号，过滤正好在第一个数字的情况
      str += num[i] + ','; //加千分位逗号
      continue;
    }
    str += num[i]; //倒着累加数字
  }
  return str
    .split('')
    .reverse()
    .join(''); //字符串=>数组=>反转=>字符串
}
```

## 字符串出现最多的三个

思路将字符串存为 Map 对象，通过 `[...map.entries()]`转为数组，通过 sort 排序，即可

## 协商缓存

浏览器在第一次访问资源时，服务器返回资源的同时，在 response header 中添加 Last-Modified 的 header，值是这个资源在服务器上的最后修改时间，浏览器接收后缓存文件和 header。
如果本地打开缓存文件，即使没有对文件进行修改，但还是会造成 Last-Modified 被修改，服务端不能命中缓存导致发送相同的资源
因为 Last-Modified 只能以秒计时，如果在不可感知的时间内修改完成文件，那么服务端会认为资源还是命中了，不会返回正确的资源。
根据文件内容是否修改来决定缓存策略——在 HTTP / 1.1 出现了 ETag 和 If-None-Match。

## transition、transform 和 animation 的区别

- Transform

  **transform 属性是静态属性，一旦写到 style 里面，将会直接显示作用，无任何变化过程,**

  transform 的主要用途是用来做元素的特殊变形。

transform 的主要用途是用来做元素的特殊变形。

- Transition

  transition: property duration timing-function delay;(过渡的属性，完成过度效果需要时间， 速度曲线，延迟时间)

- animation
  动画名称，动画执行时间，速度曲线，动画延迟时间，播放次数，是否反向播放
  animation 可以设定每一帧的样式和时间
- 区别：

  1. 触发条件不同。transition 通常和 hover 等事件配合使用，由事件触发。animation 则立即播放。
  2. 循环。 animation 可以设定循环次数。
  3. 精确性。 animation 可以设定每一帧的样式和时间。tranistion 只能设定头尾。 animation 中可以设置每一帧需要单独变化的样式属性， transition 中所有样式属性都要一起变化。
  4. 与 javascript 的交互。animation 与 js 的交互不是很紧密。tranistion 和 js 的结合更强大。js 设定要变化的样式，transition 负责动画效果，天作之合，比之前只能用 js 时爽太多。

## 伪元素

before 定位的基准是其主元素的右上角，after 定位的基准是主元素的结尾处。当主元素没有内容时 before 与 after 仍会正常工作，属性为 inline 元素，若需要设置高宽度等属性需要 `display：block;`。

## 事件委托

当事件触发时，把要做的事委托给父元素来处理。利用事件冒泡的机制。

- 事件委托的优点

  1.可以大量**节省内存**占用，减少事件注册。 2.可以实现当新增子对象时无需再次对其绑定（动态绑定事件）

- 场景

`li`标签的数量很大时，循环为每个子元素添加事件.使用事件委托只为 `ul`元素添加一个 onclick 事件处理程序。因为有事件冒泡机制，单击每个 `li`标签时，都会被这个函数处理。

- e.target 和 e.currentTarget

`e.currentTarget` 指向捕获事件的对象； `e.target` 指向发生这个事件的对象
