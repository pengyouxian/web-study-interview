# react兄弟组件传值

最近有一个场景是 Child1 组件点击让 Child2 组件里面的 state 的值发生改变,Child2 是一个公用组件，把里面的 state 值改为 props 传递，修改内容太多，容易出错，就想找其他的方法来解决兄弟组件调用方法问题，下面看代码:

Child2 是第一个子组件
```js
class Child2 extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      text: "Child2"
    };
  }
  onChange = () => {
    this.setState({
      text: "Child2 onChange"
    });
  };
  componentDidMount() {
    this.props.onRef && this.props.onRef(this);
  }

  render() {
    return <div>{this.state.text}</div>;
  }
}
```

Child1是第二个子组件，和 Child2 是兄弟组件;

```js
class Child1 extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }

  render() {
    return <div onClick={this.props.onClick}>Child1</div>;
  }
}
```

home 父组件

```js
class Home extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }
  onRef = ref => {
    this.Child2 = ref;
  };

  render() {
    return (
      <div className="home">
        <Child2 onRef={this.onRef} />
        <Child1
          onClick={() => {
            this.Child2.onChange && this.Child2.onChange();
          }}
        />
      </div>
    );
  }
}
```
__分析__

- 第一步：在Child1组件的`componentDidMount`生命周期里面加上`this.props.onRef(this)`,把Child1都传递给父组件，
- 第二步父组件里面 `<Child1 onRef={this.onRef}/> this.onRef`方法为`onRef=(ref)=>{this.child1=ref;}`;
- 第三步 Child2组件触发一个事件的时候，就可以直接这样调用`this.child1.onChange()`,Child1组件里面就会直接调用`onChange`函数,修改text为Child1 onChange;

到这里就可以实现调用兄弟组件，其实还是用父组件做了中间传递。