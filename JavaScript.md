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

- 赋值运算符 =
- 