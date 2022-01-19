## 深拷贝循环引用

使用weekmap记录引用关系

## 递归堆栈溢出

## Symbol 应用场景

- 使用[Symbol](https://so.csdn.net/so/search?q=Symbol)来作为对象属性名(key)
- 使用Symbol来替代常量
- 使用Symbol定义类的私有属性/方法

## reduce

第二个参数作为第一个参数回调的第一次调用使用，不传会多进行一次遍历。

## requestanimationframe

- 1、requestAnimationFrame 会把每一帧中的所有DOM操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率，一般来说，这个频率为每秒60帧。
- 2、在隐藏或不可见的元素中，requestAnimationFrame将不会进行重绘或回流，这当然就意味着更少的的cpu，gpu和内存使用量。

## const 与 Object.freeze()

const声明的对象可以进行改变，Object.freeze()的对象无法进行更改
