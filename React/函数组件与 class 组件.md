### 函数组件与 class 组件

**函数组件**：接收 **唯一带有数据的 “props”对象**（代表属性）与并 返回一个 **React 元素**

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

**Class组件**：在**render函数**里返回一个 **React 元素**

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

#### 函数组件和class组件的区别

**是否有状态**：有状态组件和无状态组件之间的本质区别是**有无state属性**

- **函数组件**：用构造函数创建，**没有状态state，没有this，没有生命周期函数**，是一种**“无状态组件”**；
- **class组件**：用class关键字创建，**有状态state，有this，有生命周期函数**，是一种**“有状态组件”**；

**性能**：函数组件的性能比类组件的性能要高（推荐使用函数组件）

* 因为**类组件使用的时候要实例化**，而函数组件**直接执行函数取返回结果**即可

**函数组件能够捕获渲染后的值** | **函数组件会捕获render内部的状态**

https://juejin.im/post/6844904006645448712

https://juejin.im/post/6844903825220829198

打开这个[示例](https://codesandbox.io/s/pjqnl16lm7)，里面有一个表示当前用户的下拉框以及上面实现的两个关注组件 -- 每个都渲染了一个关注按钮。

按照下面的顺序，分别操作两个的按钮：

1. **点击**其中一个 Follow 按钮
2. 在 3 秒内**改变**下拉框中选择的用户
3. **观察**弹窗中出现的文字

你会看到一个奇怪的现象：

- 函数组件中，点击 Follow Dan，然后切换到 Sophie，弹窗仍然显示‘Followed Dan’
- 类组件中，弹窗却显示‘Followed Sophie’：



在 React 中，**props 是不可变的**

- 当父组件以**不同的 props** 去渲染函数组件时，React 会重新调用其构造函数。

**但是，`this`一直都是可变的。**

* 事实上，这就是**`this`在 class 中的目的**。React 通过经常的**更新 this 来保证你在 render 和生命周期方法中总能读取到最新版本的数据**。

所以，当我们请求期间，this.props 改变了，`showMessage`方法就会从’太新‘的 props 中读取用户信息。

关于 **UI 本质的现象**。如果概念上来说 **UI 是应用当前状态的一种映射**，**那处理事件其实也是渲染的一部分结果 - 就像视觉上的渲染输出一样**。我们的处理事件其实”属于“某个特定 props 和 state 生成的特定渲染。

但是，延时任务打破了这种关联。我们的`showMessage`回调不再和特定的渲染绑定，所以就”失去“了其正确的 props。正是从 this 中读取这个行为，切断了两者之间的关联。



假如有如下组件：

```js
function ProfilePage(props) {
  const showMessage = () => {
    alert("Followed " + props.user);
  };

  const handleClick = () => {
    setTimeout(showMessage, 3000);
  };

  return <button onClick={handleClick}>Follow</button>;
}
```

```js
class ProfilePage extends React.Component {
  showMessage = () => {
    alert("Followed " + this.props.user);
  };

  handleClick = () => {
    setTimeout(this.showMessage, 3000);
  };

  render() {
    return <button onClick={this.handleClick}>Follow</button>;
  }
}
```

