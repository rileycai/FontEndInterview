#react 学习笔记

## React.Component 与 React.PureComponent 的区别
+ React.PureComponent 与 React.Component 很相似。两者的区别在于 React.Component 并未实现 shouldComponentUpdate()，而 React.PureComponent 中以浅层对比 prop 和 state 的方式来实现了该函数。
+ 如果赋予 React 组件相同的 props 和 state，render() 函数会渲染相同的内容，那么在某些情况下使用 React.PureComponent 可提高性能。
+ PureComponent 会对 props 和 state 进行浅层比较，并减少了跳过必要更新的可能性。
+ React.PureComponent 中的 shouldComponentUpdate() 仅作对象的浅层比较。如果对象中包含复杂的数据结构，则有可能因为无法检查深层的差别，产生错误的比对结果。 **仅在你的 props 和 state 较为简单时，才使用 React.PureComponent**，或者在深层数据结构发生变化时调用 **forceUpdate()** 来确保组件被正确地更新。你也可以考虑使用 immutable 对象加速嵌套数据的比较。

## React.Fragment
+ React.Fragment 组件能够在不额外创建 DOM 元素的情况下，让 render() 方法中返回多个元素。
```javascript
render() {
  return (
    <React.Fragment>
      Some text.
      <h2>A heading</h2>
    </React.Fragment>
  );
}
```
+ 你也可以使用其简写语法 <></>

## React.lazy
+ React.lazy() 允许你定义一个动态加载的组件。这有助于缩减 bundle 的体积，并延迟加载在初次渲染时未用到的组件。
```javascript
const SomeComponent = React.lazy(() => import('./SomeComponent'));
```

## 组件的生命周期
+ 当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：
1. constructor()
2. static getDerivedStateFromProps()
3. render()
4. componentDidMount()
+ 当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：
1. static getDerivedStateFromProps()
2. shouldComponentUpdate()
3. render()
4. getSnapshotBeforeUpdate()
5. componentDidUpdate()
+  当组件从 DOM 中移除时会调用如下方法：componentWillUnmount()
