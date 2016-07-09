`render()` methods are written declaratively as functions of `this.props` and `this.state`. The framework guarantees the UI is always consistent with the inputs.

Here, `componentDidMount` is a method called automatically by React after a component is rendered for the first time. The key to dynamic updates is the call to `this.setState()`.





- 组件化
  - createClass render
  - JSX:  mixing HTML tags and components we've built, 
  - 可以继承吗？
- 数据传递
  - this.props `<Comment attr="xxx">`
- 状态
  - getInitialState
  - setState
- 交互？