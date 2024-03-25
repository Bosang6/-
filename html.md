# HTML

## 标签

<!DOCTYPR> : html5 版本，必须写在第一行，在<html>标签之前，是一个声明标签

<html lang="en">: 当前html文档的实现语言

<head>: 页面最顶层，即浏览器关闭页面那块
  <title>:网页名称，即显示在浏览器顶部的名称 </title>
</head>

<meta charset="UTF-8"> :字符集设置，UTF-8为万国字符集

</meta>

 <body>:网页主页面

#### 段落标签

<h1》 -> 《h6》：1级到6级标签，依据重要性递减，即1 > 2 > 3以此类推

〈p〉: 段落标签,paragrafo

<br /〉：换行标签，单标签 ,break

#### 文本格式化标签

<strong>: 加粗

<b>:加粗

<em>：倾斜

<i>：倾斜

<del>：删除线，给文字加一条线

<s>: 删除线

<ins>：下划线

<u>:下划线

#### div 和 span

他们都是一个容器

```html
<div>.....</div>
```

大盒子，独自占网页的一行

```html
<span>.....</span>
```

小盒子，一行允许存在很多个

#### 图像和路径

```html
<img src="图像URL" />
```

单标签, src 是该标签的必要属性

图像必须和html文档放在同一个文件夹内

属性和属性之间用空格隔开

属性采用键值对形势，key="value" 属性="属性值"

img的其他属性：

- alt：在图片无法显示可显示文字

  ```html
  <img src="不存在图像" alt="文本内容"/>
  ```

  

- title：鼠标放在图像上，提示的文本

  ```html
  <img src="存在的图片" title="提示内容">
  ```

- width：图像的宽度

  ```html
  <img src="存在的图片" width="500">
  ```

- heigth：图片的高度

  ```html
  <img src="存在的图片" heigth="50">
  ```

- border：给图像设定边框，通过后续的css指定

图像标签的路径使用

同一级路径：src="img.jpg"

下一级路径：src="同级的文件夹名/img.jpg"

上一级路径：src="../img.jpg"

绝对路径：src="D:\ .........\img.jpg"

网络绝对路径：src="http://www. ....../img,jpg"

#### 超链接标签

从一个页面跳转到另外一个页面

语法：

<a herf="跳转目标" target="目标窗口的弹出方式"> 文本或图像 《/a》

#### 外部链接

```html
<a herf="http://www.baidu.com"> 百度 </a>
```

Target: 打开方式

- target="_self" 将当前页面替换
- target="_blank" 新建一个页面打开

内部链接

内部链接不需要加http，遵循路径规则

```html
<a herf="名字.html">文字/图片按钮</a>
```

空链接：herf="#"

#### 下载链接

一般以zip或exe等形式



#### 锚点链接

在当前页面中，快速的跳转到某个位置

例：<a href="#锚点名">按钮〈/a〉

```html
<div id="锚点名">XXX</div>
```

#### 注释

```html
<!--注释内容-->
```

#### 特殊字符

```html
&nbsp;	空格字符
&lt;	<
&gt;	>

```

#### 表格标签

```html
<table> 表格定义
	<tr> 行
		<th>XXX</th> 表头单元格 table head 会居中加粗
		<td></td> 每一行里的单元格
		。。。
	</tr>
	。。。
</table>
```

##### 表格属性

align=“left、center、right” 表格的位置

border="" 表格边框

cellpadding="像素值" 单元格内部内容与边框的距离

cellspacing="像素值" 单元格之间的距离，0则无距离

width="像素值" 表格宽度

heigth="像素值" 表格高度

##### 表格结构标签

```html
<thead></thead> 表格头部，在编程完毕后可以进行最小化
<tbody></tbody> 表格主体
```

#####  合并单元格

跨行合并

```html
rowspan="合并个数"
```

跨列

```html
colspan="合并个数"
```

#### 列表标签

##### 无序列表

在网页中，以 **·** 开头， <ul> 只能嵌套 <li>

<li>：是一个容器，可以随意放内容

```html
<ul>
	<li>...</li>
	<li>...</li>
	<li>...</li>
</ul>
```



##### 有序列表

以<li>的排列顺序来显示

```html
<ol>
	<li>...</li>
	<li>...</li>
	<li>...</li>
</ol>
```



##### 自定义列表

dl ：内部只允许出现 dt 和 dd

```html
<dl>
	<dt>...</dt>
	<dd>...</dd>
	<dd>...</dd>
	<dd>...</dd>
</dl>
```

#### 表单标签

用于收集用户信息，让用户填写

表单结构：

- 表单域：包含表单元素和提示信息区域
- 表单控件（元素）：按钮，输入块
- 提示信息：用于提示表单控件

##### 表单域

```html
<form action="url地址" method="提交方式" name="表单域名称">
    表单元素控件
</form>
```

属性：

- action 指定表单数据发送的目的地址
- method 提交方式，get/post 
- name 表单域名称

##### 表单控件（元素）

input 输入元素

```html
<input type="属性值" />
```

type属性值：

- button 按钮，做某件事情
- text 文本
- password 密码
- radio 单选按钮，原点，多选一
- checkbox 多选框，打勾，多选
- submit 提交按钮

```html
<input type="submit" value="免费注册"> 将提交按钮命名为"免费注册"
```

- reset 重置表单元素
- file 文件选择按钮

其他属性：

- name 表单元素的名字，radio在同一个name才能实现多选一
- value 在输入框内显示，用户输入时消失。在单选/多选按钮中，用于返回选择的内容
- checked 默认选中按钮
- maxlength 最大输入个数

##### label标签

用于绑定一个表单元素，当触发label绑定元素时，会辅助完成一些操作

```html
<label for="text1"> 用户名：</label> <input type=”text“ id="text1"/>

```

label 的 for属性，必须通过id来绑定另外一个操作

##### select下拉列表

```html
<select>
    <option>XX</option>
    <option>XX</option>
    <option>XX</option>
    <option selected="selected">XX</option>
</select>
```

selected属性，默认选择

##### textarea文本域标签

用于留言板，评论区

```html
<textarea row="3" cols="20">
	文本内容
</textarea>
```

# CSS

网页的美容师，也是一门标记语言。

让HTML专注于结构呈现，样式由CSS处理

### 语法规范

```html
选择器 {样式} //选择器为HTML内的标签

<p>文字。。。</p>

<style>
    p { 					// p标签
        color:red;
        font-size:12px;		 // 一定要分号结束
    }
</style>
```



### CSS代码风格

1. 紧凑式

```css
h3 { color: red; font-size: 20px; }
```

2. 展开式

```css
h3 {
   color: red; 
   font-size: 20px; 
}
```

### 选择器

用于选择标签

选择器分为

- 基础选择器 ：单个选择器组成
- 复合选择器

#### 标签选择器

```css
标签名 {
	属性1: 属性值1;
	属性2: 属性值2;
	属性3: 属性值3;
}
```

当某个标签多次存在时，意味着所有该名标签都会被更改

#### 类选择器

若想实现选择单独的某一个标签，可以使用类选择器

事先将类声明好后，通过标签内class属性来调用

```html
.类名 {
	属性1: 属性值1;
	属性2: 属性值2;
	属性3: 属性值3;
}

.red {
    color: red;
}

<div class="red">ciao!</div>
```

