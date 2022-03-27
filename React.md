

# React

React 是一个用于构建用户界面的 JavaScript 库。

## 基础

### jsx语法

1.  插值{}
    ```jsx
    // 1.算术运算
    const VDOM = (<h1>1+1={1+1}</h1>)  // 页面显示:1+1=2
    // 2.使用变量
    const a = 'hello world'
    const VDOM = (<h1>{a.toLocaleUpperCase()}</h1>)  // 页面显示:HELLO WORLD
    // 3.三元表达式
    const i = 2
    const VDOM = (<h1>{i == 1 ? 'True' : 'False'}</h1>) // 页面显示:False
    // 4.对象
    const girlFriend = new Object()
    girlFriend.name = 'lisa'
    const VDOM = (<h1>我的女朋友是{girlFriend.name}</h1>) // 页面显示:我的女朋友是lisa
    ```

 2. jsx中添加类名需要使用`className`

    ```jsx
    const VDOM = (<div className="one">一杯茶，一根烟，网吧坐一天</div>)
    ```
    
3. jsx使用内联样式时，需要使用{{}}，属性名必须使用驼峰命名法

   ```jsx
   const VDOM = (<div style={{color: 'red', fontSize:"100px"}}>一杯茶，一根烟，网吧坐一天</div>)
   ```

4. 只能有一个根标签
5. 标签必须闭合

### 组件

React组件分为两个类别，一个是函数式组件，一个是类式组件。

#### 函数式组件

用函数方式定义的组件，只有`props`这个参数，没有`state`和`ref`

```jsx
// 定义组件的组件名首字母一定要大写
function Demo(props){
  // 两种定义函数的方式，一种是function 函数名(){},还有一种是箭头函数const 函数名=()=>{}
  function click1(){
    console.log('function定义函数事件');
  }
  const click2 = () => {
    console.log('const定义函数事件')
  }
  return (
  	<div>
    	<h1>好好学习，天天向上。</h1>
    	<h1>{props.content}</h1>
          <button onClick={click1}>按钮1</button>
          <button onClick={click2}>按钮2</button>
          <button onClick={() => click2()}>按钮3</button>
    </div>
  )
}
ReactDOM.render(
  <Demo content={'今天读书不努力，明天睡觉打地铺'} />,
  document.getElementById('root')
);
```

#### 类式组件

用ES6类方式定义的组件。

```jsx
class MyComponent extends React.Component{
  constructor(props) {
    super(props)
    this.state = {
      name: '张三'
    }
  }
  render(){
    console.log(this); // this指向组件实例对象
    return (
      <div>
      	<h1>我是用类定义的组件</h1>
        <h1>{this.state.name}</h1>
        <h1>{this.props.content}</h1>
      </div>
    )
  }
}
ReactDOM.render(
  <MyComponent content={'今天读书不努力，明天睡觉打地铺'} />,
  document.getElementById('root')
);
```

### State状态

React推荐使用state来存储数据

```jsx
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {handsom: '帅'};
  }
  render() {
    return (
      <div>
        <h2>It is {this.state.handsome}.</h2>
      </div>
    );
  }
}
```

1.通过解构赋值的方式来简写参数

   ```jsx
   class Clock extends React.Component {
     constructor(props) {
       super(props);
       this.state = {name: '张三', age: 18};
     }
     render() {
       const {name, age} = this.state
       return (
         <div>
           <h2>我的名字：{name}，年龄：{age}.</h2>
         </div>
       );
     }
   }
   ```

2.通过setState方式来修改state

   ```jsx
   class Clock extends React.Component {
     handleClick(){
       this.setState({age: 32})
     }
     addClick(){
       this.setState({age: this.state.age+1})
     }
     constructor(props) {
       super(props);
       this.state = {name: '张三', age: 18};
     }
     render() {
       const {name, age} = this.state
       return (
         <div>
           <h2>我的名字：{name}，年龄：{age}.</h2>
           <button onClick={()=>this.handleClick()}>点我</button>
           <button onClick={()=>this.addClick()}>+1</button>
         </div>
       );
     }
   }
   ```

3.两种调用组件内部函数的方法

   1.定义时使用箭头函数，调用时使用`this.函数名`，这种方式基本不能传值，只能传递`event`事件对象，系统默认的值，或者通过子组件来传值。

   ```jsx
   class Clock extends React.Component {
     handleClick = () => {
       this.setState({age: 32})
     }
     handleChange = (e) => {
       this.setState({name: e.target.value})
     }
     constructor(props) {
       super(props);
       this.state = {name: '张三', age: 18};
     }
     render() {
       const {name, age} = this.state
       return (
         <div>
           <h2>我的名字：{name}，年龄：{age}.</h2>
           <button onClick={this.handleClick}>点我</button>
           <input type="text" value = {this.state.name} onChange={this.handleChange} />
         </div>
       );
     }
   }
   ```

   2.定义时使用常规方式定义`函数名(){}`，调用时使用箭头函数调用`()=>this.函数名()`，这种方式可以传递所有参数。传递`event`事件对象，这种系统默认值，需要在箭头函数前面的括号里填写对应的参数。

   ```jsx
   class Clock extends React.Component {
     handleClick(data){
       this.setState({age: data})
     }
     handleChange(e, data){
       this.setState({name: e.target.value, age: data})
     }
     constructor(props) {
       super(props);
       this.state = {name: '张三', age: 18};
     }
     render() {
       const {name, age} = this.state
       return (
         <div>
           <h2>我的名字：{name}，年龄：{age}.</h2>
           <button onClick={() => this.handleClick(32)}>点我</button>
           <input type="text" value = {this.state.name} onChange={(e) => this.handleChange(e, 20)} />
         </div>
       );
     }
   }
   ```

> 使用常规方式定义函数时即 `函数名(){}`，必须使用箭头函数来进行调用即 `()=>this.函数名()`，要不然函数里会取不到this的值。

### Props

props主要用于组件间传值，props和state的主要区别在于props是不可变的，而state是可以根据用户交互来改变值的。

```jsx
class Demo extends React.Component{
  constructor(props) {
    super(props);
    this.state = {sencondStage: "《React进阶》"};
  }
  render(){
    return (
      <div>
        <ul>
          <li>第一阶段：《React从入门到实战》</li>
          <li>第二阶段：{this.state.sencondStage}</li>
          <li>第三阶段：{this.props.stage}</li>
        </ul>
      </div>
    )
  }
}
ReactDOM.render(
  <Demo stage="《活着》" />,
  document.getElementById('root')
);
```

1.使用解构赋值来分解props参数

```jsx
class Demo extends React.Component{
  render(){
    const {name,age,sex} = this.props
    return (
      <div>
        <ul>
          <li>{name}</li>
          <li>{age}</li>
          <li>{sex}</li>
        </ul>
      </div>
    )
  }
}
ReactDOM.render(
  <Demo name="张三" age="18" sex="男" />,
  document.getElementById('root')
);
```

2.使用展开运算符来传递props

```jsx
class Demo extends React.Component{
  render(){
    const {name,age,sex} = this.props
    return (
      <div>
        <ul>
          <li>{name}</li>
          <li>{age}</li>
          <li>{sex}</li>
        </ul>
      </div>
    )
  }
}
ReactDOM.render(
  const p = {name: "张三", age: "18", sex: "男"}
  <Demo {...p} />,
  document.getElementById('root')
);
```

> js语法中展开运算符是不能展开对象的，但是在React中，React可以帮助我们使用展开运算符来展开props

3.对props进行限制，使用propTypes来进行类型检查，使用defaultProps来定义props的默认值

```jsx
import PropTypes from 'prop-types';
class Demo extends React.Component{
  render(){
    const {name,age,sex} = this.props
    return (
      <div>
        <ul>
          <li>{name}</li>
          <li>{age}</li>
          <li>{sex}</li>
        </ul>
      </div>
    )
  }
}
// 类型限制
Demo.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number,
  sex: PropTypes.string
}
// 默认值
Demo.defaultProps = {
  sex: '男',
  age: 45
}
ReactDOM.render(
  const p = {name: "张三", age: "18", sex: "男"}
  <Demo {...p} />,
  document.getElementById('root')
);
ReactDOM.render(
  const p1 = {name: '李四'}
  <Demo {...p1} />,
  document.getElementById('root')
);
```

对于函数式组件的props使用PropsTypes，需要注意一点，必须先声明组件，然后再导出组件

```jsx
function Clock(props){
  return (
    <div>Hello, {props.name}</div>
  )
}
Clock.propTypes = {
  name: PropTypes.string
}
export default Clock
```

### Ref

ref主要用于选取当前节点。ref其实就是标记，组件内的标签可以通过ref属性来标识自己

1.通过使用`React.createRef()`来创建ref。（推荐，官方指定）

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  handleBlur(){
    console.log('dom', this.myRef.current)
    console.log('值', this.myRef.current.value)
  }
  render() {
    return (
      <div>
        <input type="text" ref={this.myRef} onBlur={()=>this.handleBlur()}/>
      </div>
    );
  }
}
```

在函数式组件里是不能够使用ref属性的，因为它没有实例。如果想要在函数式组件中使用ref可以通过`useRef`或 `forwardRef`来实现。

```jsx
// 使用useRef
function CustomTextInput(props) {
  // 这里必须声明 textInput，这样 ref 才可以引用它
  const textInput = useRef(null);
  function handleClick() {
    textInput.current.focus();
  }
  return (
    <div>
      <input type="text" ref={textInput} />
      <button onClick={handleClick}>点我</button>
    </div>
  );
}
// 使用forwardRef
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));
// 你可以直接获取 DOM button 的 ref：
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

2.ref函数方式的使用

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = null
  }
  handleBlur(){
    console.log('dom', this.myRef.current)
    console.log('值', this.myRef.current.value)
  }
  // ref={arg => this.myRef = arg},arg代表当前节点，ref={函数}
  render() {
    return (
      <div>
        <input type="text" ref={arg => this.myRef = arg} onBlur={()=>this.handleBlur()}/>
      </div>
    );
  }
}
```

3.ref字符串方式的使用（不推荐，官方已废弃）

```jsx
class Demo extends React.Component{
  // 绑定事件处理函数
  handleClick(){
    console.log(this);//在这里，refs里面就多了ipt: input
    console.log(this.refs.ipt);  //打印我们标记的节点
    console.log(this.refs.ipt.value);  //打印我们标记的节点
  }
  // 用ref标记了这个标签，<input type="text" ref='ipt' />，然后react就会给我们存refs里面
  render(){
    return (
      <div>
        <button onClick={() => this.handleClick()}>点我点我</button>
        <input type="text" ref='ipt' />
      </div>
    )
  }
}
```

### 受控/非受控组件

受控组件就是可以被 react 状态控制的组件。在 react 中，Input textarea 等组件默认是非受控组件（输入框内部的值是用户控制，和React无关）。但是也可以转化成受控组件，就是通过 onChange 事件获取当前输入内容，将当前输入内容作为 value 传入，此时就成为受控组件。

```jsx
// 非受控组件
class Demo extends React.Component{
  constructor(props) {
    super(props);
    this.myRef = React.createRef()
  }
  handleChange(){
    console.log(this.myRef.current.value);
  }
  render(){
    return (
      <div>
        <input type="text" onChange={()=>this.handleChange()} ref={this.myRef} />
      </div>
    )
  }
}
// 受控组件
class Demo extends React.Component{
  // 定义初始状态用来存放输入框里面的值
  constructor(props) {
    super(props);
    this.state = {txt:""}
  }
  // 输入框中的值发生改变时调用
  handleChange(e){
    // console.log(e);
    this.setState({txt:e.target.value})
    // 打印this，我们就可以看到我们把表单里面的状态存进去了
    console.log(this);
  }
  render(){
    return (
      <div>
        <input type="text" value = {this.state.txt} onChange={(e)=>this.handleChange(e)} />
      </div>
    )
  }
}
```

### 组件传值

#### 父传子

1.父类子函

```jsx
class Parent extends React.Component{
  constructor(props) {
    super(props);
    this.state = {money:"100W"}
  }
  render(){
    return(
      <div className="parent">
        <h1>父组件</h1>
        <Child money={this.state.money} />
      </div>
    )
  }
}
function Child(props){
  return (
    <div className="child">
      <h1>子组件</h1>
      {props.money}
    </div>
  )
}
```

2.父类子类

```jsx
class Parent extends React.Component{
  constructor(props) {
    super(props);
    this.state = {money:"100W"}
  }
  render(){
    return(
      <div className="parent">
        <h1>父组件</h1>
        <Child money={this.state.money} />
      </div>
    )
  }
}
class Child extends React.Component{
  render(){
    console.log(this.props);
    return(
      <div className="child">
        <h1>子组件</h1>
        {this.props.money}
      </div>
    )
  }
}
```

#### 子传父

子传父思路：利用回调函数，父组件提供回调，子组件调用，将要传递的数据作为回调函数的参数，子组件通过props调用回调函数，将子组件的数据作为参数传递给回调函数。

```jsx
class Parent extends React.Component{
  getChildMsg(data){
    console.log(data)
  }
  render(){
    return(
      <div className="parent">
        <h1>父组件</h1>
        <Child getMsg={(data)=>this.getChildMsg(data)} />
      </div>
    )
  }
}
class Child extends React.Component{
  constructor(props) {
    super(props);
    this.state = {msg:"我是子组件的内容"}
  }
  handleClick(){
    this.props.getMsg(this.state.msg)
  }
  render(){
    return(
      <div className="child">
        <h1>子组件</h1>
        <button onClick={()=>this.handleClick()}>点我点我</button>
      </div>
    )
  }
}
```

## 生命周期

**挂载**

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序：constructor()、render()、componentDidMount()

**更新**

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序：render()、componentDidUpdate()。

可以通过setState和forceUpdate方法来触发更新

```jsx
class Demo extends React.Component{
  constructor(props) {
    super(props);
    this.state = {num: 0}
  }
  handleClick(){
    this.forceUpdate()
  }
  addClick(){
    this.setState({num: this.state.num + 1})
  }
  render(){
    console.log("这里调用了render这个钩子");
    return(
      <div>
        <h1>forceUpdata引起render的调用</h1>
        <button onClick={()=>this.handleClick()}>点我</button>
        <h1>setState引起render的调用</h1>
        <button onClick={()=>this.addClick()}>加一</button>
      </div>
    )
  }
}
```

**卸载**

当组件从 DOM 中移除时会调用方法：componentWillUnmount()

**错误处理**

当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用方法：static getDerivedStateFromError()、componentDidCatch()

**生命周期流程图**

![react生命周期](./img/react生命周期.png?raw=true)

## 创建项目

1.直接创建项目

```powershell
npx create-react-app my-app
```

> my-app是我们创建的项目名，前面的都是固定格式，项目名都是小写

2.使用脚手架创建项目

首先要安装脚手架

```powershell
npm i -g create-react-app
```

然后创建项目

```powershell
create-react-app demo
```

> demo是我们创建的项目名，前面的是固定的，项目名最好都是小写字母，大写字母开头会报错

通过`npm start`来启动项目

## 路由

1.安装插件

```powershell
npm install react-router-dom
或者
yarn add react-router-dom
```

### 简单使用

1.新建两个页面组件

2.需要使用路由器来包裹使用路由的地方，一般是在index.js中来包裹App根组件

```js
import ReactDOM from 'react-dom'
import App from './App'
import {BrowserRouter} from 'react-router-dom'
ReactDOM.render(
  <BrowserRouter>
  <App/>
  </BrowserRouter>
,document.getElementById('root'))
```

路由器分为BrowserRouter和HashRouter，HashRouter带#。

3.在需要进行显示的地方使用路由，一般是在App.js.

```js
import About from './Pages/About/About'
import Home from './Pages/Home/Home'
import {Link,Route} from 'react-router-dom'
class App extends React.Component{
  render(){
    return(
      <div>
        <h1>引用路由组件</h1>
        {/* 写发和写a标签一样,href=>to */}
        <Link to="/home">Home</Link>
        <Link to="/about">About</Link>
        {/* path="" 路径，对应的组件component={}  */}
        <Route path="/home" component={Home} />
        <Route path="/about" component={About} />
      </div>
    )
  }
}
```

Link组件中，to属性表示跳往页面组件的路由路径

Route组件中，path属性表示的是组件的路由路径；component属性表示的是相对应的组件，里面放的是对应的组件

#### 其他路由组件

NavLink组件是Link的一个特定版本，会在匹配上当前的url的时候给已经渲染的元素添加参数。具体属性可参考[react-router-navlink](https://reactrouter.com/docs/en/v6/api#navlink)

```js
import {NavLink,Route} from 'react-router-dom'
// render函数
/* 1.activeClassName(string) */
// 当被激活时，会添加一个active1类名
<NavLink activeClassName="active1" to="/home">Home</NavLink>
<Route path="/home" component={Home} />
/* 2.activeStyle(object) */
// 当被激活时，应用activeStyle里的样式
<NavLink
	to="/home"
	activeStyle={{
                 background: '#1890FF',
                 display: 'block',
                 width: '90px',
                 textAlign: 'center'
              }}
>
  Home
</NavLink>
<Route path="/home" component={Home} />
```

Switch组件是渲染第一个匹配到路径的组件。如果没有加Switch组件，那页面会将多个匹配到的路由全部显示。

```js
import Home from './Pages/Home/Home'
import About from './Pages/About/About'
import {NavLink,Route,Switch} from 'react-router-dom'
// render函数
/* 
	使用switch组件，只会匹配到第一个相匹配的组件
	如果有个组件的path是‘/’且它排在第一个，而link组件中的to属性是‘/home’，那么它会匹配这个第一个组件，而且不会再向下寻找了。因为switch组件的含义就是渲染匹配第一个相符合的组件。
*/
<NavLink activeClassName="active1" to="/home">Home</NavLink>
<NavLink activeClassName="active1" to="/about">About</NavLink>
<Switch>
  {/* 精准(严格)匹配和模糊匹配 */}
	<Route path="/home" component={Home} />
  <Route  path="/about" component={About} />
</Switch>
/* 未使用switch组件，只要与路径相匹配就会渲染出来 */
<NavLink activeClassName="active1" to="/about">Home</NavLink>
<Route path="/" component={Home} />
<Route  path="/about" component={About} />
// 这两个页面都会渲染出来，因为它会先匹配到‘/’，然后接着匹配到‘/about’
```

Redirect组件用于路由的重定向，它会执行跳转到对应的to路径中，一般是放在Route组件的最下方

```js
import Home from './Pages/Home/Home'
import About from './Pages/About/About'
import {NavLink,Route,Switch} from 'react-router-dom'
// render函数
// 当我们开始加载的时候,我们想一开始就给我们加载Home页面的内容
<NavLink activeClassName="active1" to="/home">Home</NavLink>
<NavLink activeClassName="active1" to="/about">About</NavLink>
<Route path="/about" component={About}/>
<Route path="/home" component={Home}/>
<Redirect to="/home" />
// 这样就会一开始就加载Home页面
```

#### 路由组件的props

路由组件和普通组件的props不太一样，路由组件的props多了history，location，match这些属性

```js
history: go(n),goBack(),goForward(),push(path, state),replace(path,state)等等参数
location: pathname："/about", search:"", state等等参数
match: params:{}, path:'/about', url:'/about'等等参数
```

普通组件和路由组件的使用方式不同

```js
import ComA from './Components/ComA/ComA'
import About from './Pages/About/About'
import Home from './Pages/Home/Home'
// render函数
/* 引用普通组件 */
<ComA/>
/* 引用路由组件 */
<Route path="/home" component={Home} />
<Route path="/about" component={About} />
```

#### withRouter函数

普通组件要是想和路由组件一样在props中拥有history，location，match这些属性，可以使用withRouter把组件给包裹起来。

eg：一般是在头部组件中有个返回按钮，点击返回，返回到之前的页面

```jsx
// ComA组件
import { withRouter } from 'react-router-dom';
class ComA extends Component {
  goHome=()=>{
    console.log(this.props);
    this.props.history.push('/home')
  }
  render() {
    return (
      <button onClick={this.goHome}>点击前往home页面</button>
    )
  }
}
export default withRouter(ComA)
```

### 嵌套路由

嵌套路由简单来说就是在路由组件中添加路由组件

```js
/* App.js */
import Home from './Pages/Home/Home'
import {Link,Route} from 'react-router-dom'
// render函数
<Link to="/home">Home</Link>
<Route path="/home" component={Home} />
/* Home组件 */
import HomeContent1 from './HomeContent1/HomeContent1'
import HomeContent2 from './HomeContent2/HomeContent2'
import { Link,Route } from 'react-router-dom'
// render函数
<Link to="/home/homeContent1">内容1</Link>
<Link to="/home/homeContent2">内容2</Link>
<Route path="/home/homeContent1" component={HomeContent1} />
<Route path="/home/homeContent2" component={HomeContent2} />
```

点击App.js中的Home会出现Home组件，再点击Home组件中的内容1就会出现HomeContent1组件。

> 在嵌套组件中最好不要在路由组件中加上`exact`属性，加上这个属性该路由组件将会开启严格匹配，这样就不能匹配到二级路由了。如果要加上这个属性，最好是在最底层的路由组件加上，因为它没有子路由组件，并不会影响二级路由组件的出现。

```js
<Route exact path="/home" component={Home} />
// exact使用的话，它的二级路由homeContent1和homeContent2都不能出现
```

**replace模式和push模式**

replace模式是替换操作

```js
// 在homeContent1中开启replace
<Route replace path="/home/homeContent1" component={HomeContent1} />
/* 操作流程：点击Home => 点击About => 点击Home => 点击C1 => 点击C2
	 点击返回操作：先返回到C1 => 返回到About => 返回到Home
	 这里是C1替换了Home
*/
```

push模式就是压栈操作，一个页面压一个页面，点击返回的时候会按执行的顺序返回

### 编程式导航

通过点击事件或者定时器等等方法跳转另一个页面的方式。

```js
this.props.history.push()--------查看（具体去哪一个页面）
this.props.history.replace()------查看（具体去哪一个页面）
this.props.history.goBack()------后退
this.props.history.goForward()---前进
this.props.history.go(n)----------前进或后退多少（参数为一个数字）
```

```jsx
/* homeContent1组件 */
goAbout(){
  this.props.history.push('/about')
}
// render函数
<button onClick={()=>this.goAbout()}>点击前往About页面</button>
```

#### 路由懒加载

定义： 懒加载简单来说就是延迟加载或按需加载,即在需要的时候的时候进行加载.

```jsx
import React, { Component,lazy,Suspense } from 'react'
const HomeContent1 = lazy(()=>import('./HomeContent1/HomeContent1'))
export default class Home extends Component {
  render() {
    return (
     <Suspense fallback={<h1>Looding我不信你看不到我Looding我不信你看不到我Looding我不信你</h1>}>
        <Route path="/home/homeContent1" component={HomeContent1}/>
        <Route path="/home/homeContent2" component={HomeContent2}/>
      </Suspense>
    )
  }
}
```

使用Suspense组件来包裹路由组件，fallback属性里面写的是路由组件加载之前显示的内容

#### 路由传参

1.传递params参数，params参数是存在props中的match里的。params参数是可以在地址栏上看到的。

```jsx
// 1.Link组件标签中，在to属性上传递参数。Route组件标签中接收路由参数
/*App.js*/
state={message:[
  {id:"01",name:"筱乐"},
  {id:"02",name:"小花花"},
  {id:"03",name:"小猪猪"},
]}
// render函数
{this.state.message.map(item=>(
  // 这里传递参数
  <Link key={item.id} to={`/home/detail/${item.id}`}>{item.name}</Link>
))}
<Route path="/home/detail/:id" component={Detail}/>
// 2.在Detail组件中通过props里的match来获取params
/*Detail组件*/
class Detail extends Component {
	state={details:["筱乐老师yyds","奖励一朵小花花","小猪猪喜欢菲菲公主"]}
  // render函数
  console.log('参数', this.props.match.params.id)
} 
```

> Route组件标签用什么接收的(:id)，那就用什么来获取(props.match.params.id)

2.传递state参数，state参数是存在props中的location里的。state参数在地址栏上是看不到的。

```jsx
// 1.Link组件标签中，to属性里传递一个对象。Route组件标签中不需要接收参数和平常一样。
/*App.js*/
state={message:[
  {id:"01",name:"筱乐"},
  {id:"02",name:"小花花"},
  {id:"03",name:"小猪猪"},
]}
// render函数
{this.state.message.map(item=>(
  <Link key={item.id} 
    to={
      {
        pathname:'/about/detail',
      	state:{id:item.id}
      }
    }
  >{item.name}</Link>
))}
<Route path="/about/detail" component={Detail} />
// 2.在Detail组件中通过props里的location来获取state
/*Detail组件*/
class Detail extends Component {
	state={details:["筱乐老师yyds","奖励一朵小花花","小猪猪喜欢菲菲公主"]}
  // render函数
  console.log('参数', this.props.location.state.id)
} 
```

3.传递search参数，search参数是存在props中的location里的。search参数在地址栏上也是可见的。

```jsx
// 1.Link组件标签中，to属性里传递一个参数，类似于query参数。Route组件标签中也不需要接收参数和平常一样。
/*App.js*/
state={message:[
  {id:"01",name:"筱乐"},
  {id:"02",name:"小花花"},
  {id:"03",name:"小猪猪"},
]}
// render函数
{this.state.message.map(item=>(
  <Link key={item.id} 
    to={`/demo/detail/?id=${item.id}`}
  >{item.name}</Link>
  <!-- 也可以写成这种形式 -->
  <Link key={item.id}
    to={{pathname:'/demo/detail', search:`id=${item.id}`}}
  >{item.name}</Link>
))}
<Route path="/demo/detail" component={Detail}/>
// 2.在Detail组件中通过props里的location来获取search
// search传递过来的是一个字符串，就是拼接在问号之后的所有东西包括问号，这里拿到的是search:"?id=01"
class Detail extends Component {
	state={details:["筱乐老师yyds","奖励一朵小花花","小猪猪喜欢菲菲公主"]}
  // render函数
  console.log('参数', this.props.location.search)
} 
```

## axios

### json-server

把一个json文件快速托管为一个服务器（提供接口），可以直接把一个json文件托管成一个具备全RESTful风格的API，并支持跨域、jsonp、路由订制、数据快照保存等功能的web服务器

1.安装

```powershell
npm install json-server -g
```

2.运行json文件

```js
json-server test.json
// 指定端口运行 (--watch:是否监听)
json-server --watch --port 3002 test.json
```

3.项目中访问接口

```js
axios.get("http://localhost:3000/handsomeBoy")
axios.get("http://localhost:3002/handsomeBoy?id=1")
axios.get("http://localhost:3000/delicateGirl")
/*----------json----------*/
{
  "handsomeBoy":[
    {
      "id":1,
      "name":"筱乐",
      "age":"18"
    },
    ...
  ],
  "delicateGirl":[
    {
      "id":4,
      "name":"TaylorSwift",
      "age":"18"
    },
    ...
  ]
}
```

### 使用

1.安装

```powershell
npm install axios
或
yarn add axios
```

2.导入

```js
import axios from 'axios'
```

3.使用

```js
// 在componentDidMount生命周期中发送网络请求，调用axios
componentDidMount(){
  axios.get("http://localhost:3000/TabBar").then(
    // 请求成功的回调
    res=>{
      console.log(res);
      console.log(response.data);
		},
    err=>{
      // 请求失败的处理
      console.log(err);
    }
  )
  /* 另一种形式 */
  axios.get("http://localhost:8000/handsomeBoy")
  // 成功的回调
  .then(res=>{console.log(res);})
  // 失败的回调
  .catch(err=>{console.log(err);})
}
```

### CURD

增加:Create 修改:Update 查找:Read 删除:Delete

1.增加POST

```js
axios.post("http://localhost:3002/handsomeBoy",{
  "name":"开心削筱乐,越削越快乐",
  "age":"1"
})
```

2.修改PUT

```js
axios.put("http://localhost:3002/handsomeBoy/1",{
  "name":"summer"
})
// 使用json-server这里还会把其他的数据给修改（清除）
```

3.更新PATCH

```js
axios.patch("http://localhost:3002/handsomeBoy/2",{
  "name":"summer123123"
})
// 使用json-server这里其他的数据就还会保存
```

4.删除DELETE

```js
axios.delete("http://localhost:3002/handsomeBoy/2")
```

5.查找GET

```js
axios.get("http://localhost:3000/TabBar")
```

### 进阶

1.创建axios实例，并配置根路径和超时时长

```js
// 1.create里面创建
let instance = axios.create({
  baseURL:"http://localhost:3000",
  timeout:2500
})
// 2.instance配置
let instance = axios.create()
instance.defaults.baseURL = "http://localhost:3000";
instance.defaults.timeout = 2500;
```

2.使用拦截器

```js
/* 如果axios没有配置默认配置，那么可以直接使用axios来设置拦截器；
	 配置了默认配置的话，请使用实例instance来设置拦截器 */
// 请求拦截器
axios.interceptors.request.use(config => {
  console.log(config)
}, err => {
  console.log(err)
})
// 响应拦截器
axios.interceptors.response.use(response => {
  console.log('响应成功的结果', response)
}, err => {
  console.log('响应失败的信息', err)
})
```

发送多个网络请求

```js
function a1(){
  axios.get("http://localhost:3000/handsomeBoy")
    .then(res=>{console.log(res)})
}
function a2(){
  axios.get("http://localhost:3000/delicateGirl")
    .then(res=>{console.log(res)})
}
axios.all([a1(),a2()])}
```

## Redux

### 概念

Redux是一个用于JavaScript状态容器，提供可预测化的状态管理。Redux可以让你构建一致化的应用，运行于不同的环境（客户端、服务器、原生应用），并且易于测试。

Redux三大核心：

1、单一数据源
		整个应用的state被存储在一棵object tree中，并且这个object tree只存在唯一一个store中（使用单一数据源，就能保证我们在每一个组件拿到这个值都是一致的）

2、State是只读的
		改变state的唯一方法就是触发action，action是一个用于描述已发生事件的普通对象（确保视图和网络请求都不能直接修改state，所有的修改必须集中化处理，并且一一按顺序进行）

3、使用纯函数来执行修改
		为了描述action如何改变state tree，你需要编写reducers（Reducers只是一些纯函数，它接收先前的state和action，并返回新的state。）
   	纯函数：一个函数的返回结果只依赖于它的参数，并且在执行过程里面没有副作用，我们就把这个函数叫做纯函数。调用这个函数，只要我参数不变，函数的值始终不变，纯函数最大的好处：可以复用。

Redux组成： 

1、State状态

全局的state

2、Action事件

- 本质就是一个JS对象

- 必须包含type

- 只是描述了有事情要发生，并没有描述如何去更新state（具体要更新state，就由reducer来做）

3、Reducer

- 本质就是函数

- 响应发送过来的action

- 函数接收两个参数，第一个是初始化state，第二个是发送过来的action

- 必须要有返回值（返回值就会给我们的Store）

4、Store

- 用来把action和reducer关联到一起的
- 可以通过createStore来构建store实例对象
- 可以通过subscribe来注册监听
- 可以通过dispatch来发送action

### 使用

1.安装redux

```powershell
npm install redux
```

2.构建action

```js
// 1.新建一个action的文件夹，在里面存放各个 action js文件
// 2.在action的js文件中书写函数，然后导出
const sendAction = () => {
  // 我们需要返回一个action对象，里面必须传递type
  return {
    type: "xiaole",
    aaa: "我是一个action"
  }
}
module.exports = {
  sendAction
}
```

3.构建reducer

```js
// 1.创建一个reducer文件夹，在里面存放一个reducer js文件
// 2.在reducer的js文件中书写，然后导出
// 定义一个初始值
const initState = {
  value: 'value值'
  aaa: "默认值"
}
/* 接受两个参数，第一个参数是state：可以定义一个初始化的state，然后进行赋值
	 第二个参数是action，通过action的type属性来确认是否执行
*/
const reducer = (state = initState, action) => {
  console.log("reduecer中", state, action); 
  // 打印出 {value:'value值',aaa:'默认值'} {type:'xiaole',aaa:'我是一个action'}
  switch (action.type) {
    case "xiaole":
      return action
    default:
      return state
  }
}
module.exports = {
  reducer
}
```

4.构建store

将`action`和`reducer`关联起来

```js
// 1.创建一个store文件夹里面存放一个store js文件
/* store.js */
// 从redux中导入createStore,用来创建store
import { createStore } from "redux";
// 导入我们创建的reducer
import { reducer } from "../reducer/reducer";
// 构建我们的store
export default createStore(reducer)
```

5.组件中使用

```jsx
/* 发送方组件 —— Home组件 */
// 引入action文件和store文件
import { sendAction } from '../../action/action'
import store from '../../store/store'
handleClick = () => {
  // 第一步，调用action（也就是我们创建的action.js中的sendAction函数）
  const action = sendAction()
  // 第二步，利用store来发送一个action
  store.dispatch(action)
}
componentDidMount(){
  // 只要reducer把值传回来了，就会触发这个subscribe监听，里面要传递一个回调函数
  store.subscribe(()=>{
    console.log("subscribe拿到值",store.getState()); 
    // 这里打印出来的就是reducer中返回的值，
    // 这个例子中reducer返回的是一个action，那返回值就是{type:"xiaole",aaa:"我是一个action"}
    // 在一个组件即是发送方又是接受方时，需要使用this.setState({})或者使用this.forceUpdate()来重新渲染组件
    this.setState({})
    // this.forceUpdate()
  })
}
// render函数
<button onClick={this.handleClick}>点我点我</button>
<h1>{store.getState().aaa}</h1>

/* 接收方组件 —— HomeCC组件 */
// 引入store文件
import store from '../../../../store/store'
// render函数
<h1>{store.getState().aaa}</h1>
```

> 整体思路：在发送方组件中调用action函数，获取到返回值，将返回值通过`store.dispatch(返回值)`将它传递到reducer函数中，并作为reducer函数的第二个参数参与reducer函数的执行，reducer函数的返回值可以通过`store.getState()`来获取。可以使用`store.subscribe()`来监听store值的变化

### 进阶

省略了action文件夹的创建，将各个action写入到需要使用到它的组件中。并且还需要多安装`react-redux`

```powershell
npm install react-redux
```

1.创建reducer文件夹，里面存放各个reducer的js文件

```js
// 定义初始值
const initState = {
  count: 10
}
exports.reducer = (state = initState, action)=>{
  // 打印传递过来的action
  console.log(action); // {type:'add_action'}
  switch (action.type) {
    case "add_action":
      // 假设我们接收的action.type值为add_action，就进行加1操作
      return {
        count: state.count + 1
      }
    default:
      return state
  }
}
```

2.创建store文件夹，里面存放一个store js文件

```js
// 从redux中导入createStore，如果有多个reducer文件导入的话需要引入combineReducers
import { createStore, combineReducers } from "redux";
// 导入创建的reducer
import { reducer } from "../reducer/reducer";
import { reducer2 } from "../reducer/reducer2";
// 有多个reducer的话，需要使用combineReducers将其整合
const allReducer = combineReducers({
  add: reducer,
  sub: reducer2
})
// add和sub都是reducer的别名
// 构建我们的store
export default createStore(allReducer)
```

3.在App.js文件中需要使用Provider组件标签来包裹想要使用store的组件。Provider接收store作为props，然后通过context往下传递，这样react中任何组件都可以通过context获取到store

```jsx
/* App.js */
import { Provider } from 'react-redux'
import store from './store/store'
import ComA from './components/comA/ComA'
import ComB from './components/comB/ComB'
// render函数
// 将store作为props传入
<Provider store={store}>
    <ComA/>
    <ComB/>
</Provider>
```

4.在发送方组件中需要使用connect函数，并且需要传递第二个参数

```jsx
/* ComA组件 */
import { connect } from 'react-redux'
class ComA extends Component {
  handleClick = () => {
    console.log("ComA",this.props);// {sendAction: f}
    // 调用sendAction，来发送我们的action
    // 只要我们发送了action，就必然会执行reducer
    this.props.sendAction()
  }
  // render函数
  console.log('ComA render', this.props)// {sendAction: f}
  <button onClick={this.handleClick}>+1</button>
}
// 注册下面传递的第二个参数sendA函数
// 该函数要有一个返回值
const sendA =(dispatch)=>{
  return{
    sendAction:()=>{
      dispatch({
        type:"add_action"
      })
    }
  }
}
// 发送方要实现connect第二个参数mapDispatchToProps，传递store.dispatch()==>分发事件
export default connect(null,sendA)(ComA)
```

5.在接收方组件中也需要使用connect函数，但它只需要传递第一个参数

```jsx
/* ComB组件 */
import { connect } from 'react-redux'
class ComB extends Component {
  // render函数
  // 这是只有一个reducer的时候获取发送方的参数
  console.log("ComB",this.props)
  // 打印出来的是 {count:10,dispathc:f dispatch(action)}
  <p>{this.props.count}</p>
  // 如果有两个reducer的话，需要指明获取获取哪个reducer中的参数
  <p>{this.props.add.count}</p>
  <p>{this.props.sub.count}</p>
}
const receiveB = (state)=>{
  console.log(state)
  // 初始化打印出来的是 {count:10}
  return state
}
// 接收方要实现connect第一个参数mapStateToProps，传递state
export default connect(receiveB)(ComB)
```

> 初始化执行顺序是：1.执行reducer 2.执行发送方组件中的sendA函数 3.执行发送方组件中的render函数 4.执行接收方组件中的receiveB函数 5.执行接收方组件中的render函数。2，3；4，5的顺序是看哪个组件在前面就先执行哪个组件里的函数。点击发送方组件中的按钮，发送事件，执行顺序是：1.执行发送方组件中的点击事件 2.执行reducer 3.执行接收方组件中的receiveB函数 4.执行接收方组件中的render组件。

## Hooks

### useState

创建状态，接收一个参数作为初始值；返回一个数组，第一个值为状态，第二个值为改变状态的函数。有了这个，函数式组件也能拥有state了。

```jsx
import React,{useState} from 'react'
export default function FuseState() {
  // 使用
  // const [state, setstate] = useState(initialState)
  // 假设不需要改变状态的方法可省略
  // const [count] = useState(0)
  const [count,setCount] = useState(0)
  const add = (n) => {
    setCount(count => count + n)
  }
  return (
  	<div>
      {count}
      <button onClick={()=>{setCount(count+1)}}>修改state</button>
	  <button onClick={() => add(2)}>点我点我</button>
    </div>
  )
}
```

### useRef

返回一个可变的ref对象，此索引在整个生命周期中保持不变。可以用来获取元素或组件的实例，用来做输入框的聚焦或者动画的触发。

```jsx
import React,{useRef} from 'react'
export default function FuseRef() {
  // 创建ref
  const myRef = useRef()
  const myRef2 = useRef()
  // 点击事件
  const handleClick = () => {
    // 函数组件没有this！！
    alert(myRef.current.value)
  }
  const handleClick2 =()=>{
    console.log(myRef2.current);
  }
  return (
    <div ref={myRef2}>
      <input type="color" ref={myRef}/>
      <button onClick={handleClick}>点我弹出输入框的内容</button>
      <button onClick={handleClick2}>打印节点</button>
    </div>
  )
}
```

### useEffect

第一次渲染完成之后都会执行一遍这个函数。

(1)第一个参数为函数，第二个参数为依赖列表，只有依赖更新时才会执行函数；当页面刷新的时候先执行返回函数再执行参数函数

 (2)如果不接受第二个参数，那么每次更新渲染页面的时候，都会调用useEffect的回调函数

```jsx
import React,{useState,useEffect} from 'react'
export default function Effect1() {
  /* 第二个参数没有的话，只要页面刷新都会执行effect */
  useEffect(() => {
    console.log('只要刷新就会执行')
  })
  /* 模拟componentDidMount，只会在第一次渲染之后执行，之后再也不会执行 */
  useEffect(() => {
    console.log('我是模拟componentDidMount');
  }, [])
  /* 模拟componentWillUnmount，返回一个函数，在组件卸载的时候执行 */
  useEffect(() => {
    return () => {
      console.log('组件卸载');
    }
  }, [])
  const unmount=()=>{
    ReactDOM.unmountComponentAtNode(document.getElementById("root"))
  }
  /* 传递第二个参数，表示监听它，当它的值发生改变的时候，触发effect */
  const [count, setCount] = useState(0)
  useEffect(() => {
    console.log('监听count');
  }, [count])
  return (
    <div>
      {/* 模拟componentWillUnmount */}
      <button onClick={unmount}>点击卸载组件</button>
      {count}
      <button onClick={()=>{setCount(count+2)}}>点击加2</button>
    </div>
  )
}
```

>useEffect第二个参数(不写)：假设我们什么都不传，就代表监听所有人；第二个参数：假设我们传递一个空数组，就代表着谁也不监听；具体我们想监视谁，我们就在第二个参数传递谁

### useReducer

```jsx
// const [state, dispatch] = useReducer(reducer, initialState, init)
// const [状态,分发] = useReducer(reducer, 初始值)
import React,{useReducer} from 'react'
export default function ReducerDemo() {
  // 定义初始状态
  const initState = {count:0}
  // 构建reducer
  const reducer = (state,action)=>{
    // 通过action.type来判断接下来的操作
    switch (action.type) {
      case 'add':
        return {count:state.count+1}
      case 'add3':
        return {count:state.count+3}
      case 'sub':
        return {count:state.count-1}
      default:
        return {count:state.count}
    }
  }
  const [state, dispatch] = useReducer(reducer, initState)
  return (
    <div>
      <h1>{state.count}</h1>
      <button onClick={()=>dispatch({type:'add'})}>点击加1</button>
      <button onClick={()=>dispatch({type:'add3'})}>点击加3</button>
      <button onClick={()=>dispatch({type:'sub'})}>点击减1</button>
    </div>
  )
}
```

### useContext

在祖组件中，将子组件类似于插槽的方式插入父组件中，并在页面上显示祖父子三个组件，并且祖组件传值子组件。

```jsx
import React, { useState } from 'react'
// 父组件
const Father =(props)=>{
  // console.log(props.children);
  return <div>{props.children}</div>
}
// 子组件
const Child =(props)=>{
  console.log(props)
  // 打印出 {name:'筱乐'}
  return <div>我是儿子---{props.name}</div>
}
// 祖组件
export default function Context() {
  const [name] = useState("筱乐")
  return (
    <div>
      <Father>
        {/* 孙组件直接放置在祖组件里面的，传值直接传，孙组件直接通过props拿 */}
        <Child name={name}></Child>
      </Father>
    </div>
  )
}
```

将子组件放在父组件中，祖组件通过父组件传值，然后父组件再传值给子组件。

```jsx
import React,{useState} from 'react'
// 父组件
const Father =(props)=>{
  // console.log('father',props);
  return <div><Child name={props.name} /></div>
}
// 子组件
const Child =(props)=>{
  // console.log('child',props);
  return <div>我是儿子---{props.name}</div>
}
// 祖组件
export default function Context() {
  const [name] = useState("筱乐")
  return (
    <div>
      <Father name={name}>
      </Father>
    </div>
  )
}
```

使用`Provider`、`Consumer`，生产者消费者模式来传值

```jsx
import React,{useState} from 'react'
const playGame = React.createContext()
// 父组件
const Father = () => {
  return <div><Child /></div>
}
// 子组件
const Child =()=>{
  // 生产者消费者模式
  return (
    <playGame.Consumer>
      {/* 通过函数的方式将Provider传递过来的值进行使用 */}
      {value => <div>我是child组件----{value}</div>}
    </playGame.Consumer>
  )
}
// 祖组件
export default function Context() {
  const [game,setGame] = useState('王者荣耀')
  return (
    <div>
      {/* 通过创建的playGame(context)的Provider(提供)，来包裹需要传值的组件，将想要传递的数据通过value属性来传递 */}
      <playGame.Provider value={game}>
          <Father/>
       </playGame.Provider>
       <button onClick={()=>{setGame("老太太深夜五杀")}}>点我秀操作</button>
    </div>
  )
}
```

通过`useContext`来使用`Provider`传递过来的值

```jsx
import React,{useState,useContext} from 'react'
// 初始化context
const playGame = React.createContext()
// 父组件
const Father =()=>{
  return <div><Child /></div>
}
// 子组件
const Child =()=>{
  // 这里通过useContext来获取Provider传来的值，没有使用Consumer来获取，方便了许多。
  const game = useContext(playGame)
  return <div>我是儿子---{game}</div>
}
// 祖组件
export default function Context4() {
  const [game,setGame] = useState('王者荣耀')
  return (
    <div>
      {/* 通过创建的playGame(context)的Provider(提供)，来包裹需要传值的组件，将想要传递的数据通过value属性来传递 */}
      <playGame.Provider value={game}>
          <Father/>
       </playGame.Provider>
       <button onClick={()=>{setGame("老太太深夜五杀")}}>点我秀操作</button>
    </div>
  )
}
```

### useCallback

使用场景：在组件中，遇到一些定义事件回调，或者获取state最新值的时候，可以选择使用useCallback来将引用固定住。

```jsx
import React, { useState,useEffect,useCallback } from 'react'
export default function Demo() {
  const [count,setCount] = useState(0)
  useEffect(() => {
    setInterval(() => {
      // setCount(count+1) // 这是异步的，获取到的值还是初始化的，当render的时候才会更新count
      setCount(count=>count+1) // 这是同步的，实时更新count的值
    }, 1000);
  }, [])
  // demo组件都会重新渲染
  console.log('demo组件重新渲染',count);
  const handleClick = useCallback(
    () => {
      console.log("useCallback",count)
      // 第二个参数，传递为空的话，获取到的就是初始化的值；传递依赖的话，获取到的就是依赖最新的值。
    },[count])
  return (
    <div>
      {count}
      <button onClick={handleClick}>按钮</button>
    </div>
  )
}
```

### useMemo

遇到一些根据已有状态来计算额外的数据的时候，并且计算的过程很消耗性能，可以使用useMemo，类似于vue中的computed

```jsx
import React, { useState,useEffect,useCallback,useMemo } from 'react'
export default function Demo2() {
  const [count,setCount] = useState(0)
  const [num,setNum] = useState(0)
  useEffect(() => {
    setInterval(() => {
      setCount(count=>count+1)
    }, 1000);
  }, [])
  useEffect(() => {
    setInterval(() => {
      setNum(num=>num+1)
    }, 50);
  }, [])
  console.log('demo组件重新渲染',count);
  const handleClick = useCallback(
    () => {
      console.log("useCallback",count)
    },[count])
  const result = useMemo(() => {
      console.log('我的调用');
      return count*100
  }, [count])
  return (
    <div>
      <h1>{count}-{result}-----{num}</h1>
      <button onClick={handleClick}>按钮</button>
    </div>
  )
}
```

