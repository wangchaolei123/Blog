## webpack中的 chunk,module,bundle的区别
- chunk代码块的类型
webpack配置中入口文件entry是chunk
入口文件以及它的依赖文件通过动态加载import()/require.ensure()出来的模块是chunk
通过SplitChunks抽取出的公有代码 也是chunk
- module：模块，在webpack眼里，任何可以被导入导出的文件都是一个模块
- bundle文件： webpack打包出来的代码文件 也可以理解为对chunk编译/压缩打包等处理后的产出
 ## webpack 构建优化
 - 优化loader配置
 - 合理使用resolve.extensions
 - 优化resolve.modules
 - 优化resolve.alias
 - 使用DLLPlugin插件
 - 使用cache-loader
 - terser启动多线程
 - 合理使用source-map
 ## webpack 设置有几种hash哈希值
1、Hash

hash是跟整个项目的构建相关，只要项目里有文件更改，整个项目构建的hash值都会更改，并且全部文件都共用相同的hash值

2、chunkhash

chunkhash，它根据不同的入口文件(Entry)进行依赖文件解析、构建对应的chunk，生成对应的哈希值。

简单来说这种是根据不同入口来配置的，比如react，react-router、resux等公共入口文件，只要这些没有改变，那么他对应生成的js的hash值也不会改变。

3、contenthash

contenthash主要是处理关联性，比如一个js文件中引入css，但是会生成一个js文件，一个css文件，

但是因为入口是一个，导致他们的hash值也相同，所以当只有js修改时，关联输出的css、img等文件的hash值也会改变，这种情况下就需要contenthash了。
 
- **Hash的作用**：
 
 1、提升webpack打包速度和项目体积：
 
 将webpack入口的chunk文件中所有公共的代码提取出来，减少代码体积；同时提升webpack打包速度。
 
 2、利用缓存机制：
 
 依赖的公共模块文件一般很少更改或者不会更改，这样独立模块文件提取出可以长期缓存。
 
 3、利用浏览器缓存
 
 方便我们在改动代码的时候，线上代码发版后及时读取最新的js文件，防止出现缓存问题；
## react fiber
- Fiber是一个JavaScript的对象。实现方式通过requestIdleCallback这一API。
- Fiber把一个渲染任务分解为多个渲染任务，而不是一次性完成，把每一个分割得很细的任务作为一个“执行单元”，任务会被分散到多个帧里面，中间可以返回至主进程控制执行其他任务。
- 为什么出现？
  react 15对主进程的超时占用。渲染组件过大，js占据主线程时间较长，任务按照先后顺序，没有任务分级。

