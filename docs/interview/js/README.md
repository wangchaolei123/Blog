## 防抖和节流

防抖(debounce)：触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发， 则重新计算时间。

场景：

1. 登录、发短信等按钮避免用户点击太快，以致于发送了多次请求，需要防抖
2. 调整浏览器窗口大小时，resize 次数过于频繁，造成计算过多，此时需要一次到位，就用到了防抖
3. 文本编辑器实时保存，当无任何更改操作一秒后进行保存

```js
    // 防抖函数
    function debounce(fn, wait) {
        let timer;
        return function() {
            let _this = this;
            let args = arguments;
            if(timer) { clearTimeout(timer) }
            timer = setTimeout(function(){
                fn.apply(_this, args)
            }, wait);      
        }
    }
    // 使用
    window.onresize = debounce(function() {console.log('resize')}, 500)

```

节流(throttle)高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率，场景：

1. `scroll` 事件，每隔一秒计算一次位置信息等
2. 浏览器播放事件，每个一秒计算一次进度信息等
3. input 框实时搜索并发送请求展示下拉列表，每隔一秒发送一次请求 (也可做防抖)

```js
    // 方式1: 使用时间戳
    function throttle1(fn, wait) {
        let time = 0;
        return function() {
            let _this = this;
            let args = arguments;
            let now = Date.now()
            if(now - time > wait) {
                fn.apply(_this, args);
                time = now;
            }
        }
    }
    // 方式2: 使用定时器
    function thorttle2(fn, wait) {
        let timer;
        return function () {
            let _this = this;
            let args = arguments;
            
            if(!timer) {
                timer = setTimeout(function(){
                    timer = null;
                    fn.apply(_this, args)
                }, wait)
            }
        }
    }

```

## JS的call，apply，bind

### 1.call的实现

```js
Function.prototype.myCall = function(object) {
	let obj = object || window;	// 如果没有传this参数，this将指向window
	obj.fn = this;				// 获取函数本身，此时调用call方法的函数已经是传进来的对象的一个属性，也就是说函数的this已经指向传进来的对象
	let arg = [...arguments].slice(1);	// 获取第二个及后面的所有参数(arg是一个数组)
	let result = obj.fn(...arg);
  delete obj.fn
	return result
}
```



### 2.apply的实现

```js
Function.prototype.myApply = function(object) {
	let obj = object || window;	// 如果没有传this参数，this将指向window
	obj.fn = this;				// 获取函数本身，此时调用call方法的函数已经是传进来的对象的一个属性，也就是说函数的this已经指向传进来的对象
	let arg = [...arguments].slice(1);	// 获取第二个及后面的所有参数(arg是一个数组)
	let result = obj.fn(arg);    // 这里不要将数组打散，而是将整个数组传进去
  delete obj.fn
	return result
}
```

### 3.bind的实现

```js
Function.prototype.myBind = function(object) {
	let obj = object || window;
	obj.fn = this;
	let arg = [...arguments].slice(1);
	return function() {
		obj.fn.apply(object, arg);
	}
}
```

##  npm 发布 package

1. 注册 npm 账号 https://www.npmjs.com/
2. 本地通过命令行 `npm login` 登陆
3. 进入到项目目录下（与 package.json 同级），在 package.json 中指定发布文件、文件夹

## js 代码压缩 minify 的原理是什么

uglify 包里有 ast.js 所以它一定是生成了抽象语法树
接着遍历语法树并作出优化，像是替换语法树中的变量，变成a，b，c那样的看不出意义的变量名。还有把 if/else 合并成三元运算符等。
最后输出代码的时候，全都输出成一行。

## CSRF 攻击

跨站请求伪造（Cross-site request forgery），也被称为 one-click attack 或者 session riding，通常缩写为 CSRF 或者 XSRF， 是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法。跟跨网站脚本（XSS）相比，XSS 利用的是用户对指定网站的信任，CSRF 利用的是网站对用户网页浏览器的信任。

## 类数组

**一个简单的定义，如果一个对象有 `length` 属性值，则它就是类数组**

```js
const arrayLike = {
  0: 3,
  1: 4,
  2: 5,
  length: 3
}
//这就是一个类数组
```



`document.getElementsByTagName`，`document.querySelectorAll` 。除了 DOM API 中，常见的 `function` 中的 `arguments` 也是类数组

## compose函数

```js
function compose() {

  let fns = [...arguments];

  return function() {
    let args = [...arguments];
    let result = fns.reduce((ret, fn) => {
      ret = fn.apply(this, ret);
      return Array.isArray(ret) ? ret : [ret];
    }, args);
    
    return result;
  }
}

let toUpperCase = (x) => x.toUpperCase();
let exclaim = (x) => x + '!';
let shout = compose(toUpperCase,exclaim);
let str = shout('hello world');
console.log(str);
```

## 深拷贝

递归实现

```js
//使用递归的方式实现数组、对象的深拷贝
function deepClone1(obj) {
  //判断拷贝的要进行深拷贝的是数组还是对象，是数组的话进行数组拷贝，对象的话进行对象拷贝
  var objClone = Array.isArray(obj) ? [] : {};
  //进行深拷贝的不能为空，并且是对象或者是
  if (obj && typeof obj === "object") {
    for (key in obj) {
      if (obj.hasOwnProperty(key)) {
        if (obj[key] && typeof obj[key] === "object") {
          objClone[key] = deepClone1(obj[key]);
        } else {
          objClone[key] = obj[key];
        }
      }
    }
  }
  return objClone;
}
```

利用WeakMap进行性能优化

```js
/**
 * 深拷贝关注点:
 * 1. JavaScript内置对象的复制: Set、Map、Date、Regex等
 * 2. 循环引用问题
 * @param {*} object 
 * @returns 
 */
function deepClone(source, memory) {
  const isPrimitive = (value) => {
    return /Number|Boolean|String|Null|Undefined|Symbol|Function/.test(Object.prototype.toString.call(value));
  }
  let result = null;

  memory || (memory = new WeakMap());
  // 原始数据类型及函数
  if (isPrimitive(source)) {
    console.log('current copy is primitive', source);
    result = source;
  }
  // 数组
  else if (Array.isArray(source)) {
    result = source.map(value => deepClone(value, memory));
  }
  // 内置对象Date、Regex
  else if (Object.prototype.toString.call(source) === '[object Date]') {
    result = new Date(source);
  }
  else if (Object.prototype.toString.call(source) === '[object Regex]') {
    result = new RegExp(source);
  }
  // 内置对象Set、Map
  else if (Object.prototype.toString.call(source) === '[object Set]') {
    result = new Set();
    for (const value of source) {
      result.add(deepClone(value, memory));
    }
  }
  else if (Object.prototype.toString.call(source) === '[object Map]') {
    result = new Map();
    for (const [key, value] of source.entries()) {
      result.set(key, deepClone(value, memory));
    }
  }
  // 引用类型
  else {
    if (memory.has(source)) {
      result = memory.get(source);
    } else {
      result = Object.create(null);
      memory.set(source, result);
      Object.keys(source).forEach(key => {
        const value = source[key];
        result[key] = deepClone(value, memory);
      });
    }
  }
  return result;
}
```

## 跨域

**协议**，**域名**，**端口**，三者有一不一样，就是跨域

1. CORS，在服务器端设置几个响应头，如 `Access-Control-Allow-Origin: *`
2. Reverse Proxy，在 nginx/traefik/haproxy 等反向代理服务器中设置为同一域名
3. JSONP

## cookie的字段

- path
- domain
- key
- value
- httpOnly
- simeSite

## node/v8 中的垃圾回收机制

1. `Scavenge`，工作在新生代，把 `from space` 中的存活对象移至 `to space`
2. `Mark-Sweep`，标记清除。新生代的某些对象由于过度活跃会被移至老生代，此时对老生代中活对象进行标记，并清理死对象
3. `Mark-Compact`，标记整理。

## new 

1. 创建了一个新对象

2. 链接到原型

3. 绑定this指向 

4. 返回这个对象

   ```js
   function _new() {
    let obj = {}
    let Con = [].shift.call(arguments)
    obj.__proto__ = Con.prototype
    let result = Con.apply(obj, arguments)
    return typeof obj === 'object' ? obj : {}
   }
   ```

   

## typeof 与 instanceof

1. `typeof` 用以**判断基础数据类型** (null 除外)
2. `instanceOf` 借助原型链**判断复杂数据类型**

## amd umd 以及cmd

### 1.amd

 RequireJS 在推广过程中对模块定义的规范化产出

采用异步方式加载模块，制定了定义模块的规则，这样模块和模块的依赖可以被异步加载，不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。

预加载,在定义模块的时候就提前加载好所有模块。

### 2.cmd

SeaJS 在推广过程中对模块定义的规范化产出。

该规范解决的浏览器环境下如何编写代码实现模块化，该规范定义可模块的一些遵循的特征，来支持能共用的模块

1. 模块单一文件
2. 不应引入模块作用域范围内的新的自由变量
3. 懒加载

### 3.umd

是一种思想，就是一种兼容 commonjs,AMD,CMD 的兼容写法，define.amd / define.cmd / module 等判断当前支持什么方式，都不行就挂载到 window 全局对象上面去

## 闭包

函数执行时会形成一个私有作用域，通常情况下当函数执行完成，栈内存会自动释放（垃圾回收）

但是如果函数执行完成，当前私有作用域(栈内存)中的某一部分内容被内存以外的其它东西(变量/元素的事件)占用了，那么当前的栈内存就不会释放掉，也就形成了不销毁的私有作用域(里面的私有变量也不会销毁)

```js
function createIncrementor(start) {
  return function () {
    return start++;
  };
}

var inc = createIncrementor(5);

inc() // 5
inc() // 6
inc() // 7

```

通过闭包，start的状态被保留了，闭包（上例的inc）用到了外层变量（start），导致外层函数（createIncrementor）不能从内存释放。

只要闭包没有被垃圾回收机制清除，外层函数提供的运行环境也不会被清除，它的内部变量就始终保存着当前值，供闭包读取，所以闭包inc使得函数createIncrementor的内部环境，一直存在

## 数组方法

不改变原数组：concat/join/reduce/map/forEach/filter/slice/findIndex

改变原数组：push/unshift/pop/shift/sort/splice/reverse
## null 和undefined
null表示 “没有对象”，即该处不应该有值。

- 作为函数的参数，表示该函数的参数不是对象。

- 作为对象原型链的终点。

undefined表示“缺少值”，就是此处应该有一个值，但是还没有定义。

- 变量被声明了，但没有赋值时，就等于undefined。

- 调用函数时，应该提供的参数没有提供，该参数就等于undefined。

- 对象没有赋值的属性，该属性的值为undefined。

- 函数没有返回值时，默认返回undefined。