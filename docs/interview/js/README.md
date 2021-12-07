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

## npm 发布 package

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
    const _new = (constructor, ...args) => {
    let obj = {}
    obj.__proto__ = constructor.prototype;
    let res = constructor.apply(obj, args);
    return res instanceof Object ? res : obj;
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

## 原型与原型链

- prototype
  在JavaScript中，每个函数都有一个prototype属性，这个属性指向函数的原型对象。
  原型的概念：每一个javascript对象(除null外)创建的时候，就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型中“继承”属性。
- __proto__
  这是每个对象(除null外)都会有的属性，叫做__proto__，这个属性会指向该对象的原型。
- constructor
  每个原型都有一个constructor属性，指向该关联的构造函数。
- 实例与原型
  当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层为止。
- 原型链
  在JavaScript中万物都是对象，对象和对象之间也有关系，并不是孤立存在的。对象之间的继承关系，在JavaScript中是通过prototype对象指向父类对象，直到指向Object对象为止，这样就形成了一个原型指向的链条，专业术语称之为原型链。

## 柯里化

```js
function add() {
    const _args = [...arguments];
    function fn() {
        _args.push(...arguments);
        return fn;
    }
    fn.toString = function () {
        return _args.reduce((sum, cur) => sum + cur)
    }
    console.log(fn, _args);
    return fn
}
```

- instanceof

```js
const myInstanceof = (left, right) => {
    const rightProto = right.prototype;
    let leftProto = left.__proto__;
    if (leftProto === null) {
        return false
    }
    if (leftProto === rightProto) {
        return true
    }
    return myInstanceof(leftProto, right);
}
```

- 排序

```js
function sort(arr) {
    let len = arr.length;
    for (let i = 0; i < len; i++) {
        for (let j = 0; j < len - j - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                let temp = arr[j + 1];
                arr[j + 1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr
}
```

- 手动实现map和foreach

```js
Array.prototype.fakeMap = function (context) {
    if (!Array.isArray(this) || typeof context != 'function') {
        return
    }
    var res = [];
    for (let i = 0; i < this.length; i++) {
        res.push(context(this[i], i, this))
    }
    return res
}
Array.prototype.myEach = function (fn, context) {
    var context = arguments[1];
    console.log(arguments);
    if (typeof fn !== "function") {
        throw new TypeError(fn + "is not a function");
    }

    for (var i = 0; i < this.length; i++) {
        fn.call(context, this[i], i, this);
    }
};
```

- Promise的all和race

```js
Promise.all = function (arr) {
    return new Promise((resolve, reject) => {
        const ans = [];
        let index = 0;
        for (let i = 0; i < arr.length; i++) {
            arr[i].then(res => {
                ans[i] = res;
                index++;
                if (index === arr.length) {
                    resolve(ans)
                }
            }).catch(err => reject(err))
        }
    })
}
Promise.race = function (arr) {
    return new Promise((resolve, reject) => {
        arr.forEach(p => {
            Promise.resolve(p).then(val => resolve(val), err => reject(err))
        })
    })
}
```

## 变量提升

js代码执行分为俩个阶段：
1.词法分析：词法分析主要包括：分析变量声明、分析函数声明、分析形参三个部分。
2.执行阶段。

- 变量提升

```js
    console.log(a);  //undefined
    var a = 123; 
```

因为变量a的声明被提到了作用域顶端。上面代码编译后应该是下面这个样子

```js
    var a;
    console.log(a)
    a = 123
    //所以输出内容为 undeifend
```

- 函数提升
  具名函数的声明有两种方式：1. 函数声明式 2. 函数字面量式。

```js
//函数声明式
function bar () {}
//变量形式声明； 
var foo = function () {}
```

所有的声明都会提升到作用域的最顶上去。

同一个变量只会声明一次，其他的会被忽略掉或者覆盖掉。

函数声明的优先级高于变量声明的优先级，并且函数声明和函数定义的部分一起被提升。

## async 与 defer

 `defer` 与 `async` 的区别如下:

* 相同点: **异步加载 (fetch)**
* 不同点:
  * async 加载(fetch)完成后立即执行 (execution)，因此可能会阻塞 DOM 解析；
  * defer 加载(fetch)完成后延迟到 DOM 解析完成后才会执行(execution)，但会在事件 `DomContentLoaded` 之前
