#react 学习笔记

## React.Component 与 React.PureComponent 的区别
+ React.PureComponent 与 React.Component 很相似。两者的区别在于 React.Component 并未实现 shouldComponentUpdate()，而 React.PureComponent 中以浅层对比 prop 和 state 的方式来实现了该函数。
+ 如果赋予 React 组件相同的 props 和 state，render() 函数会渲染相同的内容，那么在某些情况下使用 React.PureComponent 可提高性能。
+ PureComponent 会对 props 和 state 进行浅层比较，并减少了跳过必要更新的可能性。
+ React.PureComponent 中的 shouldComponentUpdate() 仅作对象的浅层比较。如果对象中包含复杂的数据结构，则有可能因为无法检查深层的差别，产生错误的比对结果。 **仅在你的 props 和 state 较为简单时，才使用 React.PureComponent**，或者在深层数据结构发生变化时调用 **forceUpdate()** 来确保组件被正确地更新。你也可以考虑使用 immutable 对象加速嵌套数据的比较。