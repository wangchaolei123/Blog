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

前端路由实现的是**监听url变化**，实现有俩种方式：hash和history模式。无需刷新页面就能加载相应的页面。Hash的url格式为 `www.a.com/#/`，当#后的哈希值发生改变 ，通过 `hashchange`事件监听，然后页面跳转.History url 通过 `history.pushState`和 `history.replaceState`改变url。

- hash只能改变#后的值，而history模式可以随意设置同源url；
- hash只能添加字符串类的数据，而history可以通过API添加多种类型的数据；
- hash的历史记录只显示之前的 `www.a.com`而不会显示hash值，而history的每条记录都会进入到历史记录；
- hash无需后端配置且兼容性好，而history需要配置 `index.html`用于匹配不到资源的情况。

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

## useMemo和useCallback

- useMemo主要用来解决使用 React hooks 产生的无用渲染的性能问题。
- useCallback
  作用：在依赖项发生变化的时候，返回一个新的函数引用。
  需求：
  1）请求数据的接口需要放在useEffect外面，因为要将该方法传递给子组件
  2）在conditions发生变化时，自动执行该函数
  3）只有在conditions发生变化时，才会重新render子组件中的内容。

## 自定义hooks

- useDebounce

```js
import { useEffect, useRef } from 'react'
const useDebounce = (fn, ms = 30, deps = []) => {
    let timeout = useRef()
    useEffect(() => {
        if (timeout.current) clearTimeout(timeout.current)
        timeout.current = setTimeout(() => {
            fn()
        }, ms)
    }, deps)
    const cancel = () => {
        clearTimeout(timeout.current)
        timeout = null
    }
  
    return [cancel]
  }
 
export default useDebounce
```

- useThrottle

```js
import { useEffect, useRef, useState } from 'react'
 
const useThrottle = (fn, ms = 30, deps = []) => {
    let previous = useRef(0)
    let [time, setTime] = useState(ms)
    useEffect(() => {
        let now = Date.now();
        if (now - previous.current > time) {
            fn();
            previous.current = now;
        }
    }, deps)
 
    const cancel = () => {
        setTime(0)
    }
  
    return [cancel]
  }
 
export default useThrottle
```

- useTitle

```js
import { useEffect } from 'react'
 
const useTitle = (title) => {
    useEffect(() => {
      document.title = title
    }, [])
  
    return
  }
 
export default useTitle
```

- useScroll

```js
import { useState, useEffect } from 'react'
 
const useScroll = (scrollRef) => {
  const [pos, setPos] = useState([0,0])
 
  useEffect(() => {
    function handleScroll(e){
      setPos([scrollRef.current.scrollLeft, scrollRef.current.scrollTop])
    }
    scrollRef.current.addEventListener('scroll', handleScroll, false)
    return () => {
      scrollRef.current.removeEventListener('scroll', handleScroll, false)
    }
  }, [])
  
  return pos
}
 
export default useScroll
```

## React新的生命周期

- getDerivedStateFromProps
  代替 `componentWillReceiveProps`。`componentWillReceiveProps()`方法判断前后两个 props 是否相同，如果不同再将新的 props 更新到相应的 state 上去。这样做一来会破坏 state 数据的单一数据源，导致组件状态变得不可预测，另一方面也会增加组件的重绘次数。
  `getDerivedStateFromProps`禁止访问 `this.props,` 确保`根据当前的 props 来更新组件的 state
- getSnapshotBeforeUpdate
  代替componentWillUpdate。在 React 开启异步渲染模式后，在 render 阶段读取到的 DOM 元素状态并不总是和 commit 阶段相同，这就导致在componentDidUpdate 中使用 componentWillUpdate 中读取到的 DOM 元素状态是不安全的，因为这时的值很有可能已经失效了。getSnapshotBeforeUpdate 会在最终的 render 之前被调用，也就是说在 getSnapshotBeforeUpdate 中读取到的 DOM 元素状态是可以保证与 componentDidUpdate 中一致的。
- 官网文档指出使用这些生命周期的代码会在未来版本的react中更容易产生 `bug`，尤其是对于异步渲染的版本
- 由于未来采用异步渲染机制，所以在17版本中去掉的生命周期钩子函数：

```
componentWillMount
```


　　 componentWillRecieveProps

　　componentWIllUpdate
