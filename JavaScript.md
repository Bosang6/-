# JavaScript基础

### JavaScript结构

```
					JavaScript
				/				  \
			ECMAScript			Web APIs
							   /	   \
							 DOM	    BOM
```



### 书写位置

#### 1.内部js

在html 内部的<body>内写<script>

```html
<body>
  <script>
  	alert('你好 js') 弹出警示框
  </script>
</body>
```

#### 2.外部js

通过src属性引入外部js

```html
<body>
  <script src="./js/my.js"></script>
</body>
```

注：引入后中间不要写内容，因为不会被执行

#### 3.内联js

在标签内部书写

```html
<body>
  <button onclick="alert('被点击')">按钮名字</button>
</body>
```

#### 注释和结束符

和c一样，但是不用写分号；，浏览器会自动推断结束位置

#### 输入和输出

- 输出语法，在页面显示，类似div

```html
<body>
  document.write('我是div标签')
  document.write('<h1>我是标题</h1>')
</body>
```

- 弹窗警告

```javascript
alert('警告！')
```

- 控制台打印，用于调试

```javascript
console.log('控制台打印')
```

- 弹窗输入

```javascript
prompt('请输入')
```

注：警示框会被先执行，其他依次执行

### 变量

变量声明

```javascript
let 变量名
```

赋值用 = 

```javascript
let 变量名
变量名 = 18
```

let 和 var 的区别

let 同一个变量名只能声明一个

var 可以声明多个相同变量

#### 变量规则

- 不能以关键字声明
- 只能用下划线，字母，数字，$ 来组成，数字不能开头
- 严格区分大小写， Age 和 age 不是一个变量
- 遵循驼峰命名，第一个单词小写，后面的单词首字母大写

### 数组

声明

```javascript
let arr = [数据1,数据2,数据3,.....,数据n]
```

若需要整个数组只需要写数组名，若使用某个需要index

### 常量

用 const 声明，声明之后不能改变，否则报错，声明时必须赋值

```javascript
const pi = 3.14
```

#### const和let 使用场景

当在声明对象和数组（也是一种对象）时，建议使用const来声明。因为他们在声明时，是以指针形式在栈（stack）中保存，内容则被保存在堆（heap）中。在对const声明的对象和数组操作/修改时，并不会因为声明为const而无法操作/修改

在能够使用const的情况下，尽量优先使用

### 数据类型

#### 基本数据类型

- number 数字型
- string 字符串：单引号和双引号没有本质区别，但是属性使用双引号，所以推荐使用单引号，可以用 + 拼接
- boolean 布尔型 true/false
- undefine 未定义型 一个变量声明且未被赋值过
- null 空类型 空，未知的一个特殊值

#### 模版字符串

用于拼接字符串和变量

```javascript
let age = 18
document.write(`我今年${age}岁了`)
```

用反引号包裹字符串，内部变量用${变量}来表示

##### Undefine 和 null 的区别

undefine 是一个未被赋值过的东西，不能做算术运算

null 是一个赋值量，允许算数运算

#### 检测数据类型

```javascript
let num = 10
typeof num
typeof(num)
```

#### 数据类型转换

##### 隐式转换

自动转换

+号 ：字符串拼接时，若一个变量为number也会转换为string

​	+号 "数字" 会自动转换为数字型

-号 ：会查看是否为number型，转换后进行减法运算

##### 显示转换

数据类型（变量名）

```javascript
let str = '123'
Number(str)
```

注：首字母大写

parseInt(变量名) 强行转换整数型

```javascript
parseInt('123sadasd') 结果也为123
parseInt('asd123asd') 这样不行，只能取数字开头的字符串
```

parseFloat(变量名) 强行转换为浮点型

### 运算符

- 变量 += 1 变量每次加1
- 变量++ / 变量--

##### 比较运算符

- == 比较左右两边的值是否相等（有隐式转换）
- === 全等，判断值和数据类型（严格判断）NaN 不等于任何数字，包括自己
- !== 不等
- 字符串比较，按ASCII来比

##### 逻辑运算符

- AND ：&&
- OR：||
- NOT：！

### 语句

##### if/else语句

```javascript
if（条件）{
	过程
}
else {
	过程
}
```

##### 三元运算符

```javascript
条件 ? 满足条件的代码 ： 不满足条件的代码
```

可以用于赋值操作

```javascript
let num = 3 < 5 ? 3 : 5
```

先判断3 < 5, 左真右假，返回给num

##### switch分支

```javascript
switch(数据){
	case 值1:
		代码1
		break
	case 值2:
		代码2
		break
	case 值3:
		代码3
		break
	default:
		代码n
		break
}
```

数据和值 的类型和值必须全部相等（===）才会执行，没有一个满足时执行default。

##### while循环

```javascript
while(条件){
	代码
}
```

continue 结束本次循环，进入下一个循环

break终止循环

##### for循环

```javascript
for(变量起始值;终止条件;变量变化){
	代码
}
```

### 数组操作

- 查：数组[下标]
- 改：数组[下标] = 新值
- 增：arr.push(新增内容到末尾，并返回该数组的新长度)

```javascript
arr.push(元素1,元素2,元素3,....,元素n)
```

- arr.unshift 将一个或多个元素添加到数组开头，返回新数组长度

```
arr.unshift(元素1,元素2,元素3,....,元素n)
```

- 删：arr.pop 删除数组最后一个元素，并返回被删除的元素

```javascript
arr.pop()
```

- arr.shift 删除第一个元素

```javascript
arr.shift()
```

- arr.splice 删除指定元素

```
arr.splice(起始位置，删除几个元素)
```

删除几个元素不写则直接删到最后一个元素

### 函数

##### 声明

执行特定任务的代码块

```javascript
function 函数名(参数列表){
	函数体
    return xxx
}
```

在某些特殊情况，可能不需要用户输入参数列表，这时需要在定义函数时，给上一个默认值

```javascript
function getSum(x = 0, y = 0){
	document.write(x + y)
}

getSum(1,2) // 会为函数赋值，改变x和y值
getSum() //使用默认值
```

##### 匿名函数

不具有函数名的函数

函数表达式：将匿名函数赋值给一个变量

```javascript
 let fn = function(){
 	函数体
 }
 
 fn() //调用函数
```

##### 具名函数 和 匿名函数 的区别

具名函数：在函数声明以后。可以在<script>标签内的任意一个位置调用 具名函数

```javascript
函数名(...) 
function 函数名(参数列表){
	函数体
    return xxx
}
函数名(...)
```

匿名函数：只能在声明后才能使用

```javascript
 let fn = function(){
 	函数体
 }
 
 fn() //声明/赋值 后使用
```

##### 匿名函数立即执行

防止变量污染，可以在立即执行函数内部随意声明，不影响全局变量

```javascript
// 版包裹式 (function(){ })()
(function 函数名(形参列表){
	函数体
})(实参列表); //立即执行函数必须要加分号，否则报错
// 全包裹式 (function(){ }())
(function (形参列表){
	函数体
}(实参列表));
```

末尾的（）本质是调用函数

立即执行函数可以写函数名

##### 逻辑中断

undefine 被定义为 false，若参数未被赋值，则默认为false

```javascript
console.log(11 && 22) //打印 22 AND运算：两侧都为真则返回右侧
console.log(11 || 22) //打印 11 OR运算：两侧都为真则返回左侧
```

##### 各类数据类型的布尔值

- String：''（空字符串为false）
- Number：0 为false
- undifined：false
- null：false
- NaN：false

空字符串/null 在进行减法运算时，为被当作0来看

```javascript
"" + 1 //结果为 "1" 字符串1
null/"" - 2 //结果为 -2 数字2
(undefined) 变量 - 1 //结果为NaN
```

### 对象

是一种无序的引用数据类型

```javascript
let 对象名 = {}
let 对象名 = new Obeject
```

对象由属性和方法组成

```javascript
let 对象名 = {
	属性1：属性值,
    属性2：属性值,
    属性3：属性值,
	方法名：函数
}
```

#### 对象使用

##### 查

```javascript
对象.属性
```

属性名可以用引号来表示，但对其属性操作时，需要用中括号

```javascript
let person = {
	'first-name':'huang',
	'last-name':'xiaobo',
	age = 18
}

console.log(person['first-name'])
```

##### 改

```javascript
对象.属性 = 新属性值
```

##### 增

JavaScript允许在对象内增加新的属性

```
对象.新属性 = 属性值
```

##### 删除（不常用）

```javascript
delete 对象.属性
```

#### 对象方法

```javascript
let person = {
	uname:'xiaobo',
    eat: function(){
        console.log('吃饭')
    },
    sleep: function(){
        console.log('睡觉')
    }
}

//调用
person.eat()
```

#### 遍历对象

打印对象内的所有属性

##### for in 语法

可以遍历数组，但不推荐，因为下标为string

```javascript
let arr = ['pink','red','green']
for(let key in arr){
	console.log(key) //打印arr的下标
	console.log(arr[key]) //打印内容
}
```

```javascript
let obj = {
	uname:'xiaobo',
	age: 18,
	gender: 'M'
}

for (let key in obj){ // key 是一个字符串！！！
	console.log(key) // 打印属性名 uname, age, gender
    console.log(obj.k) == console.log(obj.'uname') // 错！！！
    console.log(obj[k]) //正确写法
}
```

#### 内置对象Math

- random 生成0-1的随机数，左闭右开
- ceil 向上取整
- floor 向下取整
- max 找最大数
- min 找最小数
- pow 幂运算
- abs 绝对值
- round 四舍五入

```javascript
Math.函数名(参数)
```

# WEB-API

用js去操作 html 和浏览器

BOM （浏览器对象模型）

### DOM

DOM （文档对象模型）：Document Object Model 操作网页内容，实现用户交互

DOM树：将 HTML 文档以树状图结构直观表现出来

从HTML内获取一个标签，每个标签是以对象的形式传递到js中

#### 打印对象

```javascript
const div = document.querySelector('div')
console.dir(div) // div 是obj类型， 该行打印div内容
```

#### document 对象

网页的所有内容都在document对象中

document.write() ：就是document对象内的一个方法

#### 获取DOM元素

根据CSS选择器来获取DOM元素

```javascript
document.querySelector('div') // query 查询
```

在存在多个<div>时，querySelector 优先获取第一个<div>元素

##### 以id形式获取

```html
<p id="intro">这是一个段落。</p>

<script>
  const intro = document.querySelector('#intro') //一定要加引号
</script>
```

##### 获取ul列表的li

```html
<ul>
    <li>测试1</li>
    <li>测试2</li>
    <li>测试3</li>
</ul>
<script>
	const li = document.querySelector('ul li:first-child')
</script>
```

