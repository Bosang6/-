## 项目总览

### 项目创建

通过脚手架创建

```
npx create-react-app 项目名

cd 项目名
npm start 
```

在网上下载的项目需要通过 **npm install** 来安装依赖

#### index.js入口文件

该文件引入了两个关键的库，并通过ReactDOM.createRoot方法创建了一个实例，用于渲染页面

```react
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>	 //React严格模式，帮助我们debug
    <App />				//需要被渲染的App根组件
  </React.StrictMode>
);
```

#### App.js 根组件

是一个函数式组件，以JSX语法书写

## JSX语法

是Javascript和HTML的结合语法模板。

## 数据渲染

### 返回值

在JSX中，一个函数只能返回一个HTML元素。当我们需要返回多行代码时，需要用一个标签作为根标签，并用小括号包裹所有代码。

```react
function App() {
  return (
    <div>
	  ...
    </div>
  )
}
```

### 插值

在JSX中，将JavaScript的内容引入到HTML中，需要用大括号{ }进行插值

```react
function App() {

  const divConent = '标签内容'
  const divTitle = '标签标题'
  
  return (
    <div title={divTitle}>{divConent}</div>
  )
}
```

- title属性：当鼠标放在div文本上方，会显示一个小标签

### 条件渲染

通过布尔值来决定渲染内容

```react
function App() {

  const divTitle = '标签标题'
  const flag = true

  if(flag){
    let divConent = '<span>flag为true</span>'
  }
  else {
    let divConent = <span>flag为false</span>
  }
  
  return (
    <div title={divTitle}>{divConent}</div>
  )
}
```

在JSX中，变量可以直接保存HTML的元素，无需添加引号。若添加引号反而达不到我们所需的效果。

### 列表渲染

当我们渲染一个列表时，我们需要确保每一个被渲染的元素具有key属性

key属性：保证当前元素的唯一性，在使用时确保是同级唯一即可

```react
function App() {
  const list = [{ id:1, name:'小波'},	//这些数据通常从后端获得
                { id:2, name:'波桑'},
                { id:3, name:'波波'}]

  const listContent = list.map(item => (
    <li key={item.id}>{item.name}</li>
  ))
 
  return (
    <ul>{listContent}</ul>
  )
}
```

### 空标签与Fragment

在React中，当我们要返回多个同级元素的话，我们需要使用空标签或Fragment标签进行包裹。

#### 区别

- 空标签：不能有任何属性
- Fragment：允许拥有属性

区别在于，空标签不接收任何属性。当需要渲染多个同级的标签且各自有不同的key时，需要使用Fragment标签，并且Fragment标签接收需要渲染的元素key。

```react
function App() {
  const list = [{ id:1, name:'小波'},	//这些数据通常从后端获得
                { id:2, name:'波桑'},
                { id:3, name:'波波'}]

  const listContent = list.map(item => (
      <Fragment key={item.id}>		//Fragment接收元素key
        <li>{item.name}</li>
        <li>---------------------</li>
      </Fragment>
  ))
 
  return (
    <ul>{listContent}</ul>
  )
}
```

## 事件

### 事件绑定

在JSX中，当一个行为需要触发某个事件时，需要以属性的形式绑定在标签内部。

触发事件：通常是一个回调函数。

属性：必须是以驼峰的形式命名。

```react
function App() {
  function handleClick(e) {				//获取参数事件e
    console.log('按钮被点击了', e)
  }
  
  return (
    <button onClick={handleClick}>按钮</button>
  )
}
```

e：获取当前事件的所有参数，用于查看所有属性和值

### useState状态

在React中，渲染的内容不能通过直接修改变量的值完成，需要通过对渲染元素状态的修改实现。

状态：决定渲染内容的一个属性。

导包：import { useState } from "react"

错误修改

```react
function App() {
  const divContet = '我是旧内容'
  function handleClick(e) {
    divContet = '我是新的内容'	 //直接修改，页面不更新渲染内容
  }
  
  return (
    <>
      <div>{divContet}</div>
      <button onClick={handleClick}>按钮</button>
    </>
  )
}
```

正确修改

```react
import { useState } from "react";

function App() {
  const [divContet, setDivContent] = useState('我是旧内容')

  function handleClick(e) {
    setDivContent('我是新的内容')
  }
  
  return (
    <>
      <div>{divContet}</div>
      <button onClick={handleClick}>按钮</button>
    </>
  )
}
```

#### 对象形式状态

在使用useState时，状态也可以以对象的形式来使用

```react
  const [divContet, setDivContent] = useState({
    title: '我是标题',
    content: '我是内容'
  })
```

在使用set方法来改变状态时，必须将所有的内容都写上。否则，缺少的部分会直接丢失。

```react
setDivContent({
    content: '我是新内容',	//由于缺少了title，在更新完状态后，title不再被渲染
})
```

##### 展开运算符

为了避免总是重复写那些不需要被修改的状态，通过展开运算符...，并在后续接上需要修改的状态即可。

展开运算符：将对象内容（属性=值）展开到...处。

```react
function App() {
  const [divContet, setDivContent] = useState({
    title: '我是标题',
    content: '我是内容'
  })

  function handleClick(e) {
    setDivContent({
      ...divContet,				//标题被展开运算符写入此处
      content: '我是新内容',
    })
  }
  
  return (
    <>
      <div>{divContet}</div>
      <button onClick={handleClick}>按钮</button>
    </>
  )
```

#### 数组形式状态

在使用useState时，数组需要以方括号[ ]来引入。

```react
import { useState } from "react";

function App() {
  const [data, setData] = useState([
    { id:1, name:'小波'},
    { id:2, name:'波桑'},
    { id:3, name:'波波'}]
  )

  const listData = data.map(item => (
    <li key={item.id}>{item.name}</li>
  ))
  let id = 3
  function handleClick() {
    setData([
      ...data,
      {id: ++id, name: 'popo'}
    ])
  }
  
  return (
    <>
      <div>{listData}</div>
      <button onClick={handleClick}>按钮</button>
    </>
  )
}
```

##### 删除元素

通过在set方法内部调用filter方法

```react
setData(data.filter(item => item.id !== 2))
```

将id不等于2的元素提取出来，并为data重新赋值

### 表单受控绑定

在React中，由State决定在页面上渲染的内容。当我们试图使用<input>标签来进行输入时，input自带一个value属性来保存输入内容。

为了使得输入的内容能够以React形式渲染，需要将input和state进行绑定。即每更新一次输入内容，state发生改变，便于页面渲染。

#### 步骤

1. 准备一个State状态
2. 将input标签的value属性与state绑定
3. 将input标签的onChange属性绑定State的改变函数，并将通过DOM对象来获取value值，将其作为参数传递给State改变函数。

```react
import { useState } from 'react'

function App() {

  const [value, setValue] = useState('') // 1.准备一个state

  return (
    <>
      <div className="App">
        <input 
          value={value} 																//读取数据
          onChange={ (e) => setValue(e.target.value)}/> //将读取数据传递给setValue
      </div>
    </>
  );
}

export default App;
```



## 组件

在React中，组件分为两种：

- React组件
- ReactDOM组件：支持所有HTML和SVG标签

### Props

在HTML内部的属性，在React中有些许不同，因此在React中称为Props。

#### class与className

由于React使用的是JSX语法，其class关键字的使用存在歧义。

当我们希望给DOM组件设置一个类名的话，在React中，必须使用className。

```react
<div className="class"/>
```

#### style样式

在React中，style是一种键值结构，即对象的形式来设置style格式。

##### 内联

直接在标签内部书写style样式

```react
style={{
	width:100,
	height:100,
    backgroundColor: 'grey'		//驼峰形式引入
}}
```

两层花括号的意义：

- 外层：代表着嵌入Javascript语法
- 内层：CSS对象

注意：在原生CSS用-号来连接多个单词，但在React中需要以驼峰的形式引入。

##### 外嵌

通过一个变量来保存style样式，通过插值的方式引入变量。

```react
import image from './logo.svg'
function App() {
  const imgStyle = {
    width: 200,
    height: 200,
    backgroundColor: 'gray'
  }
  return (
    <div>
      <img 
      src={image} 
      alt="logo"
      className='logo'
      style={imgStyle}
      />
    </div>
  )
}
```

##### JSX展开语法

在多个标签都需要相同的属性时，每一次都分别为他们赋值显得非常的麻烦，且代码可读性差。

为了解决这个问题，JSX提供了一种展开语法。

通过一个变量来保存属性，属性和属性值之间需要用冒号来书写。

```react
import image from './logo.svg'

function App() {
  const imgData = {
    className: 'logo',
    title: 'logo',
    style: {
      width: 200,
      height: 200,
      backgroundColor: 'gray'
    }
  
  }
  return (
    <>
    <img
      src={image}
      alt="logo"	//在某些情况无法显示图片时，将文本替换为alt属性值
      {...imgData}  //JSX展开，大括号为插值，不是对象容器
    />
    </>
  )
}
```

##### JSX展开语法与展开运算符的区别

- 展开运算符必须在容器内展开
- JSX展开语法使用的大括号是Javascript的语法标记，并不是一个容器

#### 自定义组件

通常为函数组件，函数名必须以大写字母开头，防止与HTML标签混淆。

自定义组件标签的属性由props传入自定义组件

```react
function App() {
  function Article(props){
    return (
      <div>
        <h1>{props.title}</h1>
        <p>{props.content}</p>
      </div>
    )
  }
  
  return (
    <>
      <Article 
        title="标题1"
        content="内容1"
      />
      <Article 
        title="标题2"
        content="内容2"
      />
      <Article 
        title="标题3"
        content="内容3"
      />
    </>
  )
}
```

为了在组件中少些几次props，在传递props时，直接将属性名作为函数组件的参数（ES6语法），切记，一定要以对象形式传递。

```react
  function Article({title, content}){ //对象形式传递
    return (
      <div>
        <h1>{title}</h1>
        <p>{content}</p>
      </div>
    )
  }
```

##### active属性

是一个布尔值，表示当前内容是否可见。

在书写时，不需要赋值，默认为true。

##### 组件调用组件

在某些情况下，需要在一个组件内部调用另外一个组件。

```react
function Detail ({content, active}){
  return (
  <>
    <p>{content}</p>
    <p>状态:{active ? '已显示' : '已隐藏'}</p>
  </>
  )
}
function Article({title,detailData}){
  return (
    <div>
      <h1>{title}</h1>
      <Detail {...detailData}/>	//将detailData展开并传递给Detail组件
    </div>
  )
}

function App() {
  const articleData = {
    title: '标题1',
    detailData:{
      content: '内容1',
      active: true
    }
  }
  return (
    <>
      <Article 
        {...articleData}
      />
    </>
  )
}

export default App;
```

##### children属性

在标签内部书写的内容也是Props的一部分，属于children属性。

```react
<li>xxx</li> // xxx属于children属性
```

##### JSX作为Props传递（组件插槽）

组件也可以接收JSX语句。

```react
function List({children, title, footer = <p>默认底部</p>}){ //接收JSX语句（footer）
  return (
    <>
      <h2>{title}</h2>
      <ul>
        {children}
      </ul>
      {footer}
    </>
  )
}

function App() {
  return (
    <>
      <List
        title="我是列表1"
        footer={<p>我是列表1的底部</p>} //JSX语句传递
      >
        <li>列表项1</li>
        <li>列表项2</li>
        <li>列表项3</li>
      </List>
      <List
        title="我是列表2"
      >
        <li>列表项4</li>
        <li>列表项5</li>
        <li>列表项6</li>
      </List>
    </>
  )
}
```

可以为插槽设置默认值，在没有属性传入时，引用默认属性。

### 子组件向父组件传值

通过将事件绑定的属性（onActive）作为一个参数对象传到子组件内，在子组件(Detail)更改某些数据或状态时，通过传进来的属性来调用回调函数(onActive(status))，并将信息保存在回调函数内。

```react
import React, {useState} from 'react';

function Detail({onActive}) {
  const [status, setStatus] = useState(false)
  const handleClick = () => {
    setStatus(!status)
    onActive(status)
  }
  return (
    <>
      <button onClick={handleClick}>查看隐藏信息</button>
      <p style={{
        display: status ? 'block' : 'none'
      }}>Detail内容</p>
    </>
  )
}

function App() {
  function handleActive(status) {
    console.log(status)
  }
  return (
    <>
      <Detail onActive={handleActive}/>
    </>
  )
}

export default App;
```

### 同级组件信息传递

以父级组件为媒介来传递信息，通过Props一级一级向下传。这种方法很麻烦。

React中，Hook为我们提供一种方式：Context

在根组件创建一个共享信息变量，通过creatContext方法给定初始值

```react
const num = createContext(1)
```

设置完毕后，通过useContext（共享信息变量）来获取共享信息

```react
const NumFromRoot = useContext(num)
```

createContext方法创建的共享变量是一个对象，该对象含有一个Provider方法。被Provider方法包裹的组件才允许访问共享数据。

若不包裹子组件，当子组件试图使用useContext来获取共享数据时，useContext将会返回创建共享数据时设置的默认值。这个默认值可能并不是我们所需要的。

因此，当需要对一个子组件开放共享数据时，**请务必用Provider标签包裹**

```react
    <UserContext.Provider value={{ user, setUser }}>
      <UserProfile />
    </UserContext.Provider>
```

## Hooks

除了上述的useState 和useContext,还有其他6种常用的hooks

### Reducer

当我们希望通过一种方式统一管理状态，而不是通过多个useState方式来管理的话，我们可以用Reducer来统一管理。

#### 声明

创建一个Reducer函数，其参数为需要管理的状态和操作行为。通过swtich case模式来对状态进行修改。

```react
function numReducer (state, action){
  switch (action.type) {
    case "increment":
      return state + 1
    case "decrement":
      return state - 1
    default:
      throw new Error()
  }
}
```

建立完Reducer函数之后，需要在修改状态的组件内声明一个useReducer

```react
const [state(状态当前值), dispatch(触发器)] = useReducer(Reducer触发函数, 初始值)
```

#### 使用

在构造完触发器和初始值后，类似于setState方法，将触发器绑定在一个事件触发回调函数内，触发器内必须包含触发器类型type

```react
const numDecrementa = () => dispatch({type: "increment"})
function numIncrementa(){ dispatch( {type: "decrement"} )}
//type必须以对象形式包裹
```

### Ref

通过useState,我们可以改变状态的值。若希望能够记住更变状态后的前一个值的话，我们可以通过useRef来完成。

#### 声明

先声明一个变量用于保存上一个状态值。

```react
const prevCount = useRef()
```

useRef方法返回一个对象，该对象含有一个current属性。通常我们在调用更变状态的回调函数时，先将上一个状态值保存的current属性内，然后再对状态进行修改。

```react
  function handleClick(){
    prevCount.current = count
    setCount(count + 1)
  }
```

#### 使用

在调用时，切记调用差值时使用prevCount.cerrent而不是prevCount，因为prevCount是一个对象而不是属性（保存状态的变量）。

```react
<div>{prevCount.current}</div>
```

#### Ref.current

不仅能够保存状态值，它也会指向含有ref属性的HTML元素。

```react
function App() {
  const inputRef = useRef(null)

  function handleClick(){
    inputRef.current.focus()
  }

  return (
    <>
      <div className="App">
        <input type="text" ref={inputRef}></input>
        <button onClick={handleClick}>按钮</button>
      </div>
    </>
  );
}
```

当通过button按钮触发事件，引起回调函数（handleClick）调用。回调函数通过inputRef.current指向含有ref={inputRef}属性的标签，通过focus方法，使得input输入窗口变为焦点事件（高亮输入框）。

#### 父组件调用子组件函数

在默认情况下，子组件的功能不对外开放。

如果我们希望在父组件调用子组件的方法时，可以通过一些列特殊处理来达成。

1. 改变子组件的定义方式

   通过一个变量来保存一个函数表达式，该函数表达式必须由forwardRef方法来包裹，函数表达式内需要接收 props 和 ref属性

   ```react
   const Child = forwardRef(function (props, ref) {
   	...
   })
   ```

2. 子组件的函数声明方式

   在Child组件内部，提供给父组件使用的函数需要用useImperativeHandle方法来包裹。该方法需要两个参数，第一个参数为ref，第二个参数是一个函数

   ```react
   const Child = forwardRef(function (props, ref) {
   	useImperativeHandle(ref, () => ({
           myFn: () => {
               console.log('子组件提供的函数')
           }
       }))
   })
   ```

   这里需要注意第二个参数的用法。

   第二个参数是一个箭头函数，其返回一个对象，该对象含有一个变量名为myFn，该变量保存一个箭头函数（myFn）。 

   

3. 在父组件调用

   ```react
   import { useRef, forwardRef, useImperativeHandle} from 'react'
   
   const Child = forwardRef(function (props, ref) {
   
     useImperativeHandle(ref, () => ({  //用括号包裹一个函数返回的对象，该对象含有myFn函数
       myFn: () => {
         console.log("hi")
       }
     }))
   
     return (
       <div>子组件</div>
     )
   })
   
   
   function App() {
     const childRef = useRef()
   
     function handleClick(){
       childRef.current.myFn() // 调用子组件的函数
     }
   
     return (
       <>
         <div className="App">
           <Child ref={childRef}/>
           <button onClick={handleClick}>按钮</button>
         </div>
       </>
     );
   }
   
   export default App; 
   ```

### Effect

React Hook提供的一个副作用钩子。

在跟组件加载时，useEffect的内的函数会被默认执行两次。因为React开启了严格模式，来检查函数的纯度。

```react
useEffect( () => {
	console.log("hi")
})
```

#### useEffect的第二个参数

该参数为一个数组，是一个依赖数组。当数组内的状态发生变化时，useEffect内部的函数就会被执行一次

```react
import { useEffect, useState} from 'react'


function App() {

  const [count, setCount] = useState(0)

  useEffect( () => {
    console.log("hi")
  }, [count]) 		// 第二个数组参数，当count发生变化时，执行一次第一个参数的箭头函数

  function handleClick(){
    setCount(count + 1)
  }

  return (
    <>
      <div className="App">
        <div onClick={handleClick}>{count}</div>
        <button onClick={handleClick}>按钮</button>
      </div>
    </>
  );
}

export default App;

```

### Memo

在React中，当父组件需要重新渲染时，子组件也会跟着重新渲染一次。在某些时候，子组件的重新渲染是多余的，我们希望在子组件需要被重新渲染时才渲染。

为了解决这个问题，React提供了useMemo方法。该方法提供了一个可操作的缓存空间，我们可以将一些操作缓存在一个变量中。

```react
function DoSomeMath( {value} ) {
  const result = useMemo( () => {
    let result = 0
    for(let i = 0; i < 1000; i++){
      result += value * 2
    }
    return result
  }, [value])
}
```

和useEffect一样，useMemo也存在第二个参数。该参数和Effect的作用一样，当数组内的状态发生变化时，useMemo内的函数才会被执行

### CallBack

和Memo的问题类似，useMemo解决了数据缓存问题。useCallBack则解决了函数问题。（问题：当父组件被重新渲染时，子组件也会重新渲染）

#### 问题代码实例

```react
import { useState} from 'react'

function Button({onClick}){
  console.log("子组件被渲染")
  return (
    <button onClick={onClick}>子组件</button>
  )
}


function App() {

  const [count, setCount] = useState(0)

  function update(){
    setCount(count + 1)
  }

  function handleClick(){
    console.log("子组件")
  }

  return (
    <>
      <div className="App">
        <div>{count}</div>
        <button onClick={update}>按钮</button>
        <br />
        <Button onClick={handleClick}></Button>
      </div>
    </>
  );
}

export default App;
```

当点击父组件内的button按钮时，子组件也会随之渲染一次，打印“子组件”。

#### 改变自定义函数式组件形式

原函数式组件

```react
function Button({onClick}){
  console.log("子组件被渲染")
  return (
    <button onClick={onClick}>子组件</button>
  )
}
```

#### 改变函数时组件

将函数式组件改变为**记忆组件**

通过一个const变量来保存函数，并用memo方法包裹函数。

（memo方法是react自带的一个方法，需要导包）

memo方法：将包裹的函数更变为一个记忆组件，在传入的Props参数不发生变化时，该组件就不会受到外部父组件的影响

```react
const Button = memo(function ({onClick}){
  console.log("子组件被渲染")
  return (
    <button onClick={onClick}>子组件</button>
  )
})
```

#### 父组件函数问题

在父组件被重新渲染时，父组件的内部函数会被认为是一个新的函数。从而导致了传入子组件的Props发生了变化（内容不变）。

我们需要确保在父组件被重新渲染时，我们需要传给子组件的Prop/函数，不发生变化，即同一个props。

#### useCallBack解决问题

将父组件需要传出的函数作为useCallBack方法的第一个参数。第二个参数与之前的Hook函数相同，为一个依赖数组。

1. 将回调函数改为箭头函数的形式，作为useCallBack的第一个参数。
2. 通过一个变量来保存useCallBack方法

```react
  const handleClick = useCallback(() =>{
    console.log("子组件")
  },[])
```

### 自定义HOOK函数

自定义HOOK函数用于逻辑个封装的复用

需求：实现一个按钮，用于隐藏和显示一些内容。

```react
import { useState } from 'react'

function useToggle(){												//封装函数
  const [state, setState] = useState(true)
  const toggle = () => setState(!state)

  return {state, toggle}									  //返回对象
}

function App() {

  const {state, toggle} = useToggle()				//声明对象，调用useToggle获取对象

  return (
    <>
      <div className="App">
        {state && <div>This is div</div>}		//判断state是否为真，来决定是否显示
        <button onClick={toggle}>按钮</button>	//按钮触发setState回调函数
      </div>
    </>
  );
}

export default App;
```

#### 思路

1. 声明一个use开头的函数
2. 在函数体内封装可复用的逻辑
3. 把组件中需要的状态和回调，以数组或对象的形式return出去
4. 在需要这个逻辑的组件中调用use函数，通过一个对象或数组来获取状态和回调

## Redux

是一个集中状态管理工具，通过集中管理的方式管理状态。

Redux可以独立于各种框架执行，用script标签来书写。

### JavaScript形式

#### 步骤

0. 添加一个script标签，用于导入redux包

   ```html
   <script src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.min.js"></script>
   ```

   

1. 定义reducer函数：根基不同的action对象，返回不同的新state

   该函数有2个参数，第一个为State状态，第二个为action对象，该对象含有一个type属性

   函数的返回值必须是一个对象，因为我们不能直接对状态这个变量进行修改，而是通过新的对象来替换就的对象。

   ```javascript
   function reducer(state = {count:0}, action){
   	if(action.type === 'INCREMENT'){
   		return {count : state.count += 1}
   	}
   	if(action.type === 'DECREMENT'){
   		return {count : state.count -= 1}
   	}
   	return state
   }
   ```

2. 将reducer函数作为Redux.createStore的参数来生成一个state实例，将实例用一个变量保存

   ```javascript
   const store = Redux.createStore(reducer)
   ```

3. 通过实例含有的subscribe方法，该方法可以将一个函数作为参数，每当state发生改变时，该方法内的函数就会被执行

   ```javascript
   store.subscribe( () => {
        console.log('状态发生改变，我被执行了')
   })
   ```

4. store实例含有dispatch方法，该方法需要一个对象作为参数，用于传递改变状态的action对象，此参数对象必须包含type属性。

   ```javascript
   store.dispatch({
   	type: 'DECREMENT'
   })
   ```

5. 将dispatch方法于标签做事件绑定

   ```javascript
   const inBtn = document.getElementById('increment')
   inBtn.addEventListener('click', () => {
   	store.dispatch({
   	type: 'INCREMENT'
   	})
   })
   ```

6. 通过实例来渲染，实例含有一个getState方法，该方法返回一个对象，其对象含有State属性。

   通过DOM方法获取html内id为count的标签，再调用innerText属性完成修改。

   ```javascript
   store.subscribe( () => {
        document.getElementById('count').innerText = store.getState().count
   })
   ```































































## 封装库lodash

安装

```
npm i lodash
```

