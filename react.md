### 事件绑定

#### 语法

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

### React组件

组件：将浏览界面按照模块区分，每个某块定义成一个组件

在React中，一个组件是由一个函数定义的，函数内部存放了组件的逻辑结构和视图UI，渲染组件只需要把组件当成标签书写即可

声明组件

```react
function Button() {
	// 组件内部逻辑结构
	return <button>click me</button>
}
```

在根组件app中使用

```react
function App() {
  return (
  	<div className="App">
      {/* 单标签 */}
      <Button />
      {/* 双标签 */}
      <Button></Button>
    </div>
  )
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
```

这种情况下，不会引发视图变化，即页面不更新新的值

