## virtual DOM

* 虚拟DOM最大的优势在于抽象原本的渲染过程，实现了跨平台能力，不仅仅局限于浏览器的DOM，可以是安卓和IOS的原生组件，如react native。
* 牺牲部分性能的前提下，增加了可维护性，实现了对 DOM 的集中化操作，在数据改变时先对虚拟 DOM 进行修改，再反映到真实的 DOM中，用最小的代价来更新DOM，提高效率。
* 函数式UI编程
* 更好的实现SSR

虚拟 DOM 的缺点

- 首次渲染大量 DOM 时，由于多了一层虚拟 DOM 的计算，会比 innerHTML 插入慢。
- 虚拟 DOM 需要在内存中的维护一份 DOM 的副本。
- 如果虚拟 DOM 大量更改，这是合适的。但是单一的，频繁的更新的话，虚拟 DOM 将会花费更多的时间处理计算的工作。所以，如果你有一个DOM 节点相对较少页面，用虚拟 DOM，它实际上有可能会更慢。但对于大多数单页面应用，这应该都会更快。

## react 中 ref

- 对Dom元素的焦点控制、内容选择、控制
- 对Dom元素的内容设置及媒体播放
- 对Dom元素的操作和对组件实例的操作

## 使用 SSR，可以在 componentWillMount 中访问 localStorage吗

不可以，componentWillMount还未挂载，代码仍然在服务器中执行，此时没有浏览器环境，访问localstorage报错

## useCallback 使用场景
作为props传递的函数，集合memo一起使用；
作为更新触发的依赖项。主要目的是为了避免高昂的计算和不必要的重复渲染

## setState 

react16 1.setstae 以后会把update队列加入到mount 里面 如果他在生命周期里面 其实他是进行批量去更新的 如果他是在生命周期里面去set 其实数据同步的。如果想要拿到最新的就需要脱离react生命周期，和react事件流 比如在setTimeout里面set值 他拿到的数据就是最新的。

## React的router原理

前端路由实现的是**监听url变化**，实现有俩种方式：hash和history模式。无需刷新页面就能加载相应的页面。Hash的url格式为`www.a.com/#/`，当#后的哈希值发生改变 ，通过`hashchange`事件监听，然后页面跳转.History url 通过`history.pushState`和`history.replaceState`改变url。

- hash只能改变#后的值，而history模式可以随意设置同源url；
- hash只能添加字符串类的数据，而history可以通过API添加多种类型的数据；
- hash的历史记录只显示之前的`www.a.com`而不会显示hash值，而history的每条记录都会进入到历史记录；
- hash无需后端配置且兼容性好，而history需要配置`index.html`用于匹配不到资源的情况。

## 性能优化

- 代码分割
- React.memo()、shouldComponentUpdate()等防止不必要的渲染
- Fragments避免额外标记
- 错误边界避免组件在出错时破坏整个应用

## dva中的@connect

connect是dva的一个函数，用来绑定model和view的

```js
import { connect } from 'dva';

function mapStateToProps(state) {
  return { todos: state.todos };
}
connect(mapStateToProps)(App);
```

其中connect函数有俩个参数，mapStateToProps可以理解为model层，而App为view层。总之接收一个函数，返回一个函数。

第一个函数会注入全部的models，你需要返回一个新的对象，挑选该组件所需要的models。