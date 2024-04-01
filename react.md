## 初步了解React

### 网页图标

在浏览器初次加载网页时，会在根目录寻找favicon.ico图标。

若图标不存在，第一次加载后没有找到，会报错。但第二次加载浏览器不会报错，默认图标不存在。

### 库的引入

1. react.development.js ：react开发包
2. react-dom.development.js ：react操作虚拟DOM包 
3. babel.min.js ：翻译JSX到JS

依次引入到html中

```react
<div id="test"></div> // 准备一个容器，用于渲染

<body>
	<script type="text/javascript" src="../js/react.development.js"></script>
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <script type="text/javascript" src="../js/babel.min.js"></script>
</body>
```

### 创建react的虚拟DOM （JSX方式）

```react
<script type="text/babel"> // babel翻译器
    const VDOM = <h1>Hello React</h1>
    ReactDOM.render(VDOM,document.getElementById('test')) // 获取容器id用于渲染
</script>
```

### 创建react的虚拟DOM（原生JS方式）

原生JS不需要再导入babel翻译器

语法

```react
const VDOM = React.createElement(标签名，标签属性（可以是多个）,标签内容)
```

例：

```react
<script type="text/javascript">
    const VDOM = React.createElement('h1',{id:'title'},'Hello React')
</script>
```

注意：

- 一定要用容器来渲染DOM
- script标签内的type属性必须为babel，即jsx翻译器
- 再jsx中，变量声明可以引用标签，与js不同，但需要借助babel翻译器来完成

### 为什么使用JSX？

设想这种情况，在h1标签内嵌套一个span标签

#### JSX方式

```react
<script type="text/babel"> // babel翻译器
    const VDOM = (			//可以用小括号包裹起来，看起来更像html
    <h1>
        <span>Hello React</span>
    </h1>)
</script>
```

#### JS方式

```react
<script type="text/javascript">
    const VDOM = React.createElement('h1',{id:'title'},Reacte.createElement('span',{},'Hello React'))
</script>
```

可以看出，JSX可以以html的形式来创建虚拟DOM，但原生JS需要多层嵌套React.createElement来实现，更加繁琐。

### 真实DOM与虚拟DOM

虚拟DOM：

- 本质是一个Object类型的对象
- 虚拟DOM比较‘轻’，真是DOM比较‘重’。 即真实DOM的属性值非常多，而虚拟DOM包含的属性少。因为虚拟DOM是React内部所使用的DOM，无需真实DOM的多属性。
- 虚拟DOM最终会被React转化为真实DOM，渲染在页面上

## JSX语法规则

1. 定义虚拟DOM时，不要写引号
2. 标签中引入**JS表达式**要用 { }
3. 样式的类名指定不能用class，要用className
4. 内联样式，要用style={{key:'value'}}形式来写
5. 只能用一个根标签，否则报错
6. 标签必须闭合
7. 标签首字母：
   1. 如果小写字母开头，则将该标签转为html中的同名元素，若html无同名元素（标签），则报错
   2. 若大写字母开头，react渲染对应组件，如果组件没有定义，则报错

```react
<style>
	.title{
        background-color: orange;
    }	// 可写在index.css中，然后再App.js中通过className属性来传递
</style>
-----------------------------------------------------------
<script type="text/babel"> 
    const myId = 'HELLOID'
    const myData = 'Hello,React'
    const VDOM = (
    <div>		// 只能有一个根标签，否则报错
        <h1 className="title" id={myId.toLowerCase()}> // 不能像html那样用class=""
            <span style={{color:'white',fontSize:'29px'}}>{myData}</span> //内联样式
        </h1>
        <Good>123</Good> //报错
    </div>
    )
</script>
```

### JS表达式、JS语句（代码）

表达式：会产生一个值，可以放在任何一个需要值的地方

语句：控制代码走向，if，for，switch。。



### 事件绑定

#### 语法1（React推荐）

```html
on + 事件名称 = { 事件处理程序 }
```

首先声明一个事件处理函数

```javascript
const handleClick = () => {
  console.log('button被点击了')
}
```

将处理函数绑定到一个按钮

```html
<button onClick={handleClick}>click me</button>
```

#### 语法2

```react
<button id="b1">按钮1</button>

<script>
	const b1 = document.getElementById('b1')
    b1.addEventListener('click',() =>{
        alert('按钮1被点击了')
    })
</script>
```

#### 语法3

```react
<button id="b2">按钮2</button>

<script>
	const b2 = document.getElementById('b1')
    b2.onClick = () =>{
        alert('按钮2被点击了')
    }
</script>
```



### 事件对象

是一个包含于**事件发生相关的所有信息**的一个对象，例如一个事件发生，用户点击鼠标，敲击键盘等。可以作为一个参数传递给处理该事件的函数。

#### 获取事件对象

只需要在处理事件回调函数加入一个参数

```react
const handleClick = (e) => {
  console.log('button被点击了', e)
}
```

#### 传递自定义参数

在调用事件处理函数时，格式必须为箭头函数形式

（） => 函数名 = （自定义参数）=> {}

```react
const clickHandler = (name) => {
  console.log('button被点击'，name)
}

return <button onClick={() => clickHandler('xiaobo')}>click me</button>
```

#### 自定义参数和事件对象同时获取

在事件函数被调用时，将事件参数e作为参数来调用回调函数

```react
const clickHandler = (name, e) => {
  console.log('button被点击'，name, e)
}

return <button onClick={(e) => clickHandler('xiaobo')}>click me</button>
```

### React函数式组件

组件：用于实现局部功能效果的代码和资源的合集

在React中，一个组件是由一个函数定义的，函数内部存放了组件的逻辑结构和视图，渲染组件只需要把组件当成标签书写即可

声明函数式组件

```react
function Button() { //函数名必须为大写
	// 组件内部逻辑结构
    console.log(this); //此处this是undefined,因为babel在翻译时会开启严格模式
	return <button>click me</button>
}
```

在根组件App中使用

```react
function App() {
  return (
  	<div className="App">
      {/* 单标签 */}
      <Button />
      {/* 双标签 */}
      <Button></Button>
      ReactDOM.render(<Button/>,document.getElementById('App'))
    </div>
  )
}
```

React.render(...)执行后发生了些什么：

1. React解析组件标签，找到了Button组件
2. 发现组件是使用函数定义的，随后调用该函数，函数将返回一个虚拟DOM给React，React再将虚拟DOM转化为真实DOM，输出到页面上

### 类的使用

在JavaScript中，可以像Java一样编写自己的类

```react
<script>
	class Person{
        //构造器
        constructor(name,age){
            this.name = name
            this.age = age
        }
        
        //类方法, 存放在类的原型对象上，供实例使用
        speak(){ 
            console.log('我是${this.name},今年${this.age}岁')
        }
    }
    const p1 = new Person('xiaobo',18)
    p1.speak
</script>
```

### 类式组件

在react中，自定义（创建组件）必须继承 React. Component 类，类中必须声明render方法。

```react
class 自定义组件名 extends React.Component {
    render(){
        return <h1>我是复杂的类组件</h1>
    }
}
```

当写完组件后，通过ReactDOM.render方法来调用组件

```react
ReactDOM.render(<自定义组件名>，document.getElementById('渲染位置的ID'))
```

该方法执行时都发生了些什么：

- React解析组件标签，找到组件
- React发现这个组件时一个类组件，new出来一个该类（组件）实例，并通过该实例调用类原型的render()方法
- 将render返回的虚拟DOM转换为真实DOM，并渲染到页面上

## 组件实例的三大核心

### state

State（状态），影响着组件向页面输出的内容。例如：布尔型状态，我们可以设计为 True 输出内容， False不输出。

在设计组件时，我们需要通过构造器来传递State

```react
class Weather extends React.Component{
    constructor(props){				//三大核心之一，暂且认为必须要写
        super(props)				//通过super来实现props继承
        this.state = {isHot:false}	 //state以obj对象形式来赋值
    }
    render(){
        const {isHot} = this.state
        return <h1>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
    }
}
```

#### React类中，this和方法问题

```react
class Weather extends React.Component{
    constructor(props){				
        super(props)				
        this.state = {isHot:false}	 
    }
    render(){
        const {isHot} = this.state
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
    }
    changeWeather(){
        //该方法在Weather的原型对象上，供实例使用
        //由于该方法时onClick的回调函数，在点击h1标题时，onClick直接从堆空间中提取出该函数来直接调用，而并非通过实例.changeWeather()执行。在类中的方法都默认开启了局部严格模式，导致该方法的this为undifined
        console.log(this)
    }
}
```

#### 解决方法

通过在构造器内部声明一个属性，用于保存需要被调用的回调函数。通过bind方法来实现绑定。

bind方法：

- 生成一个新的函数
- 将生成的函数绑定到bind方法的第一个参数（一个实例对象）

```react
class Weather extends React.Component{
    constructor(props){				
        super(props)				
        this.state = {isHot:false}
        this CW = this.changeWeather.bind(this)
        //this.changeWeather找到原型对象内的方法，通过bind来绑定到当前实例对象中，并用CW变量来保存bind返回的新函数（新函数就是changeWeather）
    }
    render(){
        const {isHot} = this.state
        return <h1 onClick={this.CW}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
    }
    changeWeather(){
        console.log(this)
    }
}
```

#### state修改

state不可以直接更改，必须要借助React内部的API修改

直接更改：错错错！！！！！！

```react
const isHot = this.state.isHot
this.state.isHot = !isHot
```

setState修改：正确

```react
const isHot = this.state.isHot
this.setState({isHot = !isHot})
```

#### 执行过程

```react
class Weather extends React.Component{
    constructor(props){		//在执行ReactDOM.render时，执行1次
        super(props)				
        this.state = {isHot:false, wind:'微风'}
        this CW = this.changeWeather.bind(this)
    }
    render(){		//在执行完构造器后执行1次，随后每次修改state后执行
        const {isHot} = this.state
        return <h1 onClick={this.CW}>今天天气很{isHot ? '炎热' : '凉爽',{wind}}</h1>
    }
    changeWeather(){	//每次修改state，执行1次
    	const isHot = this.state.isHot
		this.setState({isHot = !isHot})
    }
}

ReactDOM.render(<Weather/>,document.getElementById('test'))
```

#### 精简写法

不写构造器，直接声明一个state变量

不在类中声明方法，而是通过一个变量来保存箭头函数。即：

变量 = （） => { 箭头函数体 }

```react
class Weather extends React.Component{
	state = {isHot:false, wind:'微风'}
    
    render(){		
        const {isHot} = this.state
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽',{wind}}</h1>
    }
    
    changeWeather = ()=>{ 	// changeWeather变量来保存箭头函数
    	const isHot = this.state.isHot
		this.setState({isHot = !isHot})
    }
}

ReactDOM.render(<Weather/>,document.getElementById('test'))
```

#### state总结

state必须以对象形式赋值 ：state = {.....}

若要使用回调函数：

- 使用构造器并且通过bind方法绑定
- 或者使用 变量 = （） => { 箭头函数体 }

### Props

在通过ReactDOM.render（..）渲染页面时，可以在标签内添加一些属性，会传递到由React new出来的实例对象内的Props属性

```react
class Person extends React.Component{
    render(){
        const {name,age,sex} = this.props
        return (
        	<ul>
            	<li>姓名：{name}</li>
                <li>年龄：{age}</li>
                <li>性别：{sex}</li>
            </ul>
        )
    }
}

ReactDOM.render(<Person name="xiaobo" age="18" sex="男"/>，document.getElementById('test'))
```

#### 展开运算符

##### 在原生JavaScript中

展开运算符无法展开实例对象

...：用于展开数组

```react
<script type="texr/javascript">
	let arr1 = [1,3,5,7,9]
    let arr2 = [2,4,6,8,10]
    
    let arr3 = [...arr1,...arr2]	//将arr1和arr2内的内容展开并保存到arr3
</script>
```

通过数组reduce方法进行数组元素求和

```react
function sum(...number){
	return numbers.reduce((preValue,currentValue)=>{
        return preValue + currentValue
    })
}

console.log(sum(1,2,3,4,5,6))
```

也可以用来复制对象，也可以修改内容

```javascript
let person1 = {name:'xiaobo',age:18}
let person2 = {...person1}  //复制

let person3 = {...person1, name:'bobo'} //修改其中的内容
```



##### 在React中

由于React是由JavaScript和Babel共同工作，所以在React中允许通过展开运算符来展开对象，但**仅限于标签内！！！**

这种行为称为：批量传递props 或 批量传递标签属性

```react
const p = {name:'xiaobo', age:18, sex:'男'}
ReactDOM.render(<Person {...p}/>,document.getElementById('test'))
```

标签内部，number和string的区分

```react
ReactDOM.render(<Person name="xiaobo" age="18"/>,document.getElementById('test1'))	//字符串18

ReactDOM.render(<Person name="xiaobo" age={18}/>,document.getElementById('test1'))		//数字18
```



#### Props的限制

对传递的标签提出三个限制：

- 类型的限制
- 必要性的限制
- 默认值

在新版React中，prop-types.js需要额外导入，不再是React的默认包

```react
<script type="text/javascript" src="../js/prop-types.js"></script>
```

在引入prop-types.js包后，便可以在自定义组件类中，对属性进行限制

```react
class Person extends React.Component{
    render(){
        const {name,age,sex} = this.props
        return (
        	<ul>
            	<li>姓名：{name}</li>
                <li>年龄：{age}</li>
                <li>性别：{sex}</li>
            </ul>
        )
    }
}
Person.propTypes = {	//这里的p小写
    name:PropTypes.string.isRequired, //PropTypes 区分大小写！！必传（isRequired）
    sex:PropTypes.string,	//限制字符串
    age:PropTypes.number,	//限制数字
    speak:PropTypes.func,	//限制函数
}

ReactDOM.render(<Person name="xiaobo" age="18" sex="男"/>，document.getElementById('test'))
```

PropTypes.string.isRequired：name必须以string形式传递，否则报错，但任然会渲染界面

##### Props默认值

在原有代码的基础上加上一个 类.defaultProps

```react
Person.defaultProps = {
    sex:'男'
}
```

当ReactDOM.render()内部没有传递sex属性时，页面将会以男为默认值进行渲染

值得注意的是，prop里的内容**只允许读**，**不允许修改**

##### Props简写

上面的限制条件是外部添加的，为了使得组件更加统一，我们应该把defaultProps 和propTypes放入组件类中，通过添加static关键字完成。

```react
class Person extends React.Component{
    static propTypes = {	//这里的p小写
        name:PropTypes.string.isRequired, //PropTypes 区分大小写！！必传（isRequired）
        sex:PropTypes.string,	//限制字符串
        age:PropTypes.number,	//限制数字
        speak:PropTypes.func,	//限制函数
    }
	static defaultProps = {
    	sex:'男'
	}
    render(){
        const {name,age,sex} = this.props
        return (
        	<ul>
            	<li>姓名：{name}</li>
                <li>年龄：{age}</li>
                <li>性别：{sex}</li>
            </ul>
        )
    }
}
ReactDOM.render(<Person name="xiaobo" age="18" sex="男" speak={speak}/>，document.getElementById('test'))

function speak(){
    console.log('说话')
}
```





























### useState

是一个React Hook函数，允许我们向组件添加一个**状态变量**，从而控制组件的渲染效果

即一旦状态变量发生变化，组件也会随之变化

语法

```react
import { useState } from 'react' //导包

const [状态变量, set状态变量] = useState(状态变量初始值)
```

例：通过一个按钮实现自动迭代增加

```react
function App() {
  // 状态变量声明
  const [count, setCount] = useState(0)
  // 点击按钮的回调函数
  const handleClick = () => {
    setCount(count + 1)
  }
  return (
  <div>
      <button onCLick={handleClick}>{count}</button> 
   </div>
  )
}
```

### 修改状态规则

状态不可变：状态只可读，当我们需要改变状态时，只能替换状态，不可以修改它

### 修改对象规则

关于对象类型的状态变量，应传给set方法一个**全新的对象**来进行修改

直接修改不替换:

```react
const [form, setFrom] = useState({
  name: 'xiaobo',
})

const handleChangeName = () => {
  form.name = 'bobo'
}

return (
	<div>
    	<button onClick={handleChangeName}>修改</button>
    </div>
)
```

这种情况下，点击按钮不会引发视图变化，即页面不更新新的值

正确修改：新对象替换老对象

```react
const [form, setFrom] = useState({
  name: 'xiaobo',
})

const handleChangeName = () => {
  setForm({
      ...form,
      name:'bobo'
  })
}
    
 return (
	<div>
    	<button onClick={handleChangeName}>修改</button>
    </div>
)

```

