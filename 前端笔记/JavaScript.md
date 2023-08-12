# JavaScript学习笔记

## 1.javascript

### 1.1ECMAScript

（核心语言功能）：类型，语法，语句，关键字，保留字，操作符，对象

### 1.2DOM文档对象模型

（操作网页容）：多节点结构（删除，添加替换，修改）

### 1.3BOM浏览器对象模型

（与浏览器交互）：操作，访问浏览器窗口/框架

## 2.数据类型

### 2.1基本类型

#### 1.Number

整数，浮点数，科学计数，二进制，八进制，十六进制，NaN

数据类型转换：Number(变量)，parseInt(变量)，parseFloat(变量)，非加号（变量  -0 ，/1，*1）

#### 2.String

字符串类型

数据类型转换：变量.tostring，String(变量)，加号（变量 + ‘ ’）  **注意：转换前，先判定变量不是Undfined或Null**

#### 3.Boolean

true/false

在js中，只有''，0，null，undefined，NaN，这些事false，其余都是true

#### 4.Undfined

声明了，但是未赋值

#### 5.Null

明确变量未来会赋值对象类型，先赋值Null

```js
let a = null
console.log(typeof a)//结果为object
```

#### 6.Symbol

### 2.2复杂类型

#### 1.Object

#### 2.Function

#### 3.Array

#### 4.Set

#### 5.Map

#### 6.Date

#### 7.RegExp

#### 8.Null Object

## 3.运算符

## 4.流程控制

## 5.函数

var声明变量 和 声明式函数

预解析：js是一个解释型语言，就是在代码执行之前，先对代码进行解读和解释，然后再执行代码

​				也就是说，我们的js代码在运行的时候，会经历两个环节 **解释代码** 和 **执行代码**

## 6.作用域

什么是作用域，就是一个变量可以生效的范围

变量不是在所有地方都是可以使用的，而这个变量的使用范围就是作用域

### 6.1全局作用域

全局作用域是最大的作用域

在全局作用域中定义的变量可以在任何地方使用

页面打开的时候，浏览器会自动给我们生成一个全局作用域window

这个作用域会一直存在，直到页面关闭就销毁了（刷页面重置）

```js
//下面两个变量都是存在在全局作用域下面的，都是可以在任意地方使用的
var num = 100
var num2 = 200
```



### 6.2局部作用域

局部作用域就是在全局作用域下面有开辟出来的一个相对小一些的作用域

在局部作用域中定义的变量只能在这个局部作用域内使用

**在  Js 中只有函数能生成一个局部作用域，别的都不行**

每一个函数，都是一个局部作用域

```js
//这个num是一个全局作用下的变量，在任何地方都可以使用
var num = 100

function fn(){
 //下面这个变量是一个fn局部作用内部的变量
 //只能在fn函数内部使用
 var num2 = 200
}
fn()
```

### 6.3访问规则（就近）

- 当我想获取一个变量的值的时候，我们管这个行为叫做 **访问**

- 获取变量的规则：

  - 首先，在自己的作用内部查找，如果有，就直接拿来使用

  - 如果没有，就去上一级作用域查找，如果有，就拿来使用

  - 如果没有，就继续去上一级作用域查找，一次类推

  - 若果一直到全局作用域都没有这个变量，那么就会直接报错（该变量is not defined）

- 变量的访问规则，也叫做 作用域的查找机制
- 作用域的查找机制只能是向上找，不能向下找

```js
var num = 100

function fn(){
	var num2 = 200
	
	function fun(){
		var num3 = 300
		console.log(num3) //自己作用域内有，拿过来用 
		console.log(num2) //自己作用域内没有，就去上一级，就是fn的作用域里面找，发现有，拿过来用
		console.log(num) //自己这里没有，去上一级fn那里也没有，再上一级到全局作用域，发现有，直接用
		console.log(z) //自己没有，一级一级找上去到全局都没有，就会报错
		
	}
}
```

### 6.4赋值规则

- 当你想给一个变量赋值的时候，那么就先要找到这个变量，在给他赋值
- 变量赋值：
  - 先在自己作用域内部查找，有就直接赋值
  - 没有就去一级作用域内部查找，有就直接赋值
  - 还没有再去上一级作用域查找，有就直接赋值
  - **如果一直找到全局变量都没有，那么就把这个变量定义为全局变量，再给他赋值**

## 7.对象数据类型（复杂数据类型）

- 对象就是一个键值对的集合

  - 字面量的方式创建一个对象

    ```js
    var obj0 = {}
    ```

  - 内置构造函数

    ```js
    var obj2 = new Object()
    ```

​		 		**删除属性：delete**		语法为 `delete object.property` 或 `delete object['property']`

- 对象的遍历

  ```js
  for （var i in obj）{
  	//获取key
  	console.log(i)
  	//获取value
  	console.log(obj[i])
  }			
  ```

## 8.不同数值类型的存储					

- 栈：主要存储基本数据类型的内容
- 堆：主要存储复杂数据类型的内容

### 8.1基本数据类型在内存中的存储情况

- var num = 100， 在内存中的存储情况
- 直接在 **栈空间** 内存储一个数据

### 8.2复杂数据类型在内存中的存储情况

- 下面这个 对象 的存储

  ```js
  var obj = {
  name : 'Jack',
  age : 18,
  gender : 'M'
  }
  ```

- 复杂数据类型的存储
  - 在堆里面开辟一个存储空间
  - 把数据存储到存储空间
  - 把存储空间的地址赋值给栈里面的变量

## 9.数组数据类型（复杂数据类型）

数组的基本操作；

### 9.1数组的length 可读可写

- 长度length，可读可写，可影响数组的长度 
- 索引
- 遍历
- 数组常用方法

### 9.2数组去重复

[...new Set(arr)]

## 10.字符串（基本数据类型）

- 长度length，只读
- 索引/下标
- 常用方法

## 11.Json

Json转Object：JSON.parse(str)

Object转Json：JSON.stringify(obj)

## 12.模板字符串

反引号：**``**

```js
var str =`My name is ${变量}`
```

## 13.数字类型

常用方法：

```js
//Number toFixed() 返回是字符串
3.1415926.toFixed(2) //返回字符串：3.14

//Math对象

//random //0-1 包含0不包含1
Math.random()  

//round 四舍五入
Math.round(4.46)

//ceil向上取整 floor向下取整
Math.ceil(4.3) / Math.floor(4.3)

//abs绝对值
Math.abs(-10.2)

//sqrt 平方根9
Math.sqrt(9)

//pow(底数，指数)
Math.pow(2,3) //结果8

//max（多个参数） 返回最大 / （多个参数） 返回最小
Math.max(10,50,20,43) 返回最大

//PI
Math.PI
```

## 14.Date对象

js 提供的内置构造函数，专门获取时间

初始化对象：

```js
let date = new Date()

//1个传参数 毫秒数
let date1 = new Date(1691430329000) //返回 2023-08-07T17:45:29.000Z

//2个参数，3个参数 。。。
let date2 = new Date(2023,0,3，10，10，10) //年，月，日，时，分，秒

//字符串
let date3 = new Date('2023/10/10 08:08:08')



```

常用方法：

```js
var date = new Date()

//获取
//getFullYear
date.getFullYear()

//getMonth 0-1===>1-12
date.getMonth(1)

//getDate  日期
date.getDate(1)

//getDay 周几
date.getDay()

//getHours  getMinutes  getSeconds  getMilliseconds
date.getHours()

//getTime 时间戳
date.getTime()

//设置
//setFullYear  setMonth setDate  setHours setMinutes setSecondes 
date.setFullYear(2025)

//setTime
date.setTime(1691463425731)



```

## 15.定时器

在js里面，有倒计时定时器，间隔定时器

- 倒计时定时器
  - 设置：setTImeout
  - 清理：clearTimeout(定时器)
- 间隔定时器
  - 设置：setInterval
  - 清理：clearInterval(定时器)

## 16.BOM

- BOM（Browser Oobject Model）：浏览器对象模型
- 其实就是操作浏览器的一些能力
- 我们可以操作哪些内容
  -  获取一些浏览器的相关信息（窗口的大小）
  - 操作浏览器进行页面跳转
  - 获取当前浏览器地址栏的信息
  - 操作浏览器的滚动挑
  - 浏览器的信息（浏览器的版本）
  - 让浏览器出现一个弹出框（alert/confirm/prompt）
  - ...
- BOM的核心就是window对象
- window时浏览器内置的一个对象，里面包含着操作浏览器的方法

### 16.1获取浏览器窗口的尺寸

- innerHeight和innerWidth 	

  ```js
  let height = window.innerHeight
  let width = window.innerWidth
  ```

### 16.2弹出框（alert/confirm/prompt）----会造成阻塞！！

### 16.3浏览器的地址信息

- 在window中有一个对象叫location

- 就是专门用来储存浏览器的地址栏内的信息的

  **location.href**

  - Location.href 这个属性存储的事浏览器地址内url地址的信息

    ```js
    console.log(window.location.href)
    ```

  - 会把中文变成url编码的格式

  - location.href这个属性也可以给他赋值

    ```js
    window.loaction.href = './index.html'
    ```

  **location.reload**

  - Location.reload() 这个方法会重新加载一遍页面，相当于刷新，就是一个道理

### 16.4浏览器的事件

- **onload**

  - 这个不再是对象，而是一个事件
  - 是在页面所有资源加载完毕后执行的（DOM，视频，图片）

  ```js
  //挂载window下面
  window.onload = function(){
  	console.log('页面已经加载完毕了')
  }		
  ```

- **onresize**

- **onscroll**

  - 滚动时，触发这个事件

    ```js
     window.onscroll = function () {
        console.log(document.documentElement.scrollTop || document.body.scrollTop)
    }
    ```

  - scrollTo:滚动到指定位置

    - window.scrollTo(0,0)
    - window.scrollTo({left:0,top:0})

### 16.5浏览器打开关闭标签页

- window.open() 打开新的标签页
- window.close() 关闭当前标签页

### 16.6浏览器的历史记录

- window 中有一个对象叫做history

- 是专门用来存储历史记录信息的

  - history.back 是用来回退历史记录的，就是回到前一个页面，就相当于浏览器上的回退按钮

  ```js
  history.back()
  
  history.go(1)
  ```

  - History.forword 是去到一下历史记录里面，也就是去到下一个页面，就相当于浏览器上的前进按钮

  ```
  history.forward()
  
  history.go(-1)
  ```

### 16.7本地存储 

- localStorage  

  - 只能存字符串，不能存对象（可以先把对象转换成json字符串）
  - 永久存储，关闭页面不会丢失

  ```js
  localStorage.setItem('name','chris')
  localStorage.getItem('name')
  localStorage.removeItem('name')
  localStorage.clear()
  ```

- sessionStorage  

  - 只能存字符串，不能存对象（可以先把对象转换成json字符串）
  - 会话存储，关闭页面就丢失

  ```
  sessionStorage.setItem('name','chris')
  sessionStorage.getItem('name')
  sessionStorage.removeItem('name')
  sessionStorage.clear()
  ```

  

## 17.DOM

- DOM（Document Object Model）：文档对象模型
- 其实就是操作html中的标签的一些能力
- 我们可以操作哪些内容
  - 获取一个元素
  - 移除一个元素
  - 创建一个元素
  - 向页面里面添加一个元素
  - 给元素绑定一些事件
  - 获取元素的属性
  - 给元素添加一些css样式
  - ...
- DOM的核心对象就是document对象
- document对象是浏览器内置的一个对象，里面存储着专门用来操作元素的各种方法
- DOM：页面中的标签，我们通过js获取到以后，就是把这个对象叫做**DOM对象**

### 17.1获取一个元素

- ```js
  document.documentElement
  document.head
  document.body
  
  //返回 伪数组
  document.getElementById('btn');
  document.getElementsByClassName('btn');
  document.getElementsByTagName('li')
  //返回一个对象
  document.querySelector('#btn') 
  //返回 伪数组
  document.querySelectorAll('#btn')
  ```

### 17.2操作一个元素

- 操作原生属性
  - DOM节点. 属性名=‘？？？？’
- 操作自定义属性： setAttribute /getAttribute /removeAttribute  
  - li列表中可能用到自定义属性
  - 自定义属性名：data-xxxx :    设置：detaset.xxxx='？？？？'     获取：*.dataset
- 删除属性（**注：和js删除对象的方法一样**）
  - delete  DOM节点. 属性名

### 17.3操作元素文本内容

- innerHTML ：所有内容
- innerText ：文本内容，不解析HTML
- value ：表单标签的value值

### 17.4操作元素样式

- 行内样式方法：
  - DOM节点.style.height
  - DOM节点.style['background-color']
  - DOM节点.style['backgroundColor']
  - 直接赋值，即可改变样式
- 内部样式，外部样式，行内样式    getComputedStyle() 获取，但是不能赋值
  -  getConputedStyle(DOM节点)       注意：先获取DOM节点

### 17.5操作员数类名

- DOM节点.className
  - 可以直接赋值
- DOM节点.classList
  - DOM节点.classList.add('class1')   //添加
  - DOM节点.classList.remove('class1') //删除
  - DOM节点.classList.toggle('class1')  //切换，没有就添加，有就删除

### 17.6DOM节点

DOM的节点一般分为常用的三大类 	**元素节点/文本节点/属性节点**

- 元素节点
- 属性节点
- 文本节点
- 注释，/n 也是节点

**childNodes属性**	 vs  **children**

- childNodes属性：所有节点（包含文本，注释，/n）
- children：所有元素

**firstChild** vs **firstElementChild**

- 第一个节点

- 第一个元素节点

**firstChild** vs **firstElementChild**

- 最后一个节点
- 最后一个元素节点

**previousSibing** vs **previousElementSibling**

- 上一个兄弟节点
- 上一个元素节点

**nextSibing** vs **nextElementSibling**

- 下一个兄弟节点
- 下一个元素节点

**parentNode** vs **parentElement**

- 父节点
- 父元素

**DOM元素节点.attributes 拿到所有属性节点**

### 17.7操作DOM节点

- 操作无非就是 增查改删（CRUD）

  - createElement：用于创建一个元素节点
  - createTextNode：用于创建一个文本节点

  - 节点.appendChild(xxxx)
  - 节点.insertBefore(新的节点，谁的前面)

  - 节点.removeChild(节点对象)
  - 节点.remove  //删除自己以及后代节点
  - 节点.replaceChild(新的节点，老的节点)
  - 克隆节点()：false 不克隆后代，默认false
    - 节点.cloneNode(true)

### 17.8获取元素尺寸

- offsetWidth,offsetHeight

​		offet*= (border+padding+content)  //1，返回值是数字  2，box-sizing (border+padding+content) 

- clientWidth,clientHeight

​		client*=（padding+content）		

### 17.9获取元素的偏移量

- offsetLeft，offsetTop

​    	参考点：定位父级，如果父级都没有定位，偏移量相对于body

- clientLeft，clientTop

   参考点：左上角边框

### 17.10获取可视窗口的尺寸

- innerWidth，innerHeigjt  //获取窗口看的尺寸，包含滚动条

- document.documentElement.clientWidth

  document.documentElement.clientHeight

  //获取窗口看的尺寸，减去滚动条尺寸

## 18.事件

### 18.1事件组成

- 事件的组成：

  - 触发谁的事件：事件源
  - 触发什么事件：事件类型
  - 触发以后做什么：事件处理函数

  ```js
  //dom0类型的绑定方式，重复绑定的时候，后面会覆盖前面
  let　oDiv = document.querySelector('div') 
  
  oDiv.onclick = function(){}
  //触发谁的事件 => oDiv => 这个事件的事件源就是oDiv
  //触发什么事件 => onclick => 这个事件类型就是click
  //触发以后做什么 => function(){} => 这个事件的处理函数
  
  //dom0事件解绑 dom节点.onclick = null
  
  ----------------------------------------------------------------------------------------
  //dom2 绑定多个事件处理函数，按照顺序执行
  box.addEventListener('click',function(){}) //'click'事件类型
  box.addEventListener('click',function(){})
  box.addEventListener('click',function(){})
  box.addEventListener('click',function(){})
  //dom2兼容性问题，旧版浏览器可能
  box2.attachEvent('onclick',function(){})
  
  //dom2事件解绑
  box.removeEventListener('click',function(){})
  
  //dom2兼容性 解绑
  box2.detachEvent('onclick',function(){})
  ```

### 18.2事件类型

- 浏览器事件：load，scroll。。。
- 鼠标事件：click，dblclick，contextmenu，mousedown，mousekey，mousemove，mouseenter，mouseleave。。
- 键盘事件：keyup，keydown，kepress。。。  //对表单对象绑定
- 表单事件change，input，submit。。。 //对表单对象绑定
- 触摸事件：touchstart，touchend，touchmove  //移动端

### 18.3事件对象

- 什么是事件对象？

- 就是当你触发了一个事件以后，你点在哪个位置了，坐标是多少

- 例如

  - 你触发一个点击事件的时候，你点在哪个位置了，坐标是多少
  - 你触发一键盘事件的时候，你按的是哪个按钮
  - 。。。

- 每一个事件都会有一个对应的对象来描述这些信息，我们把这个对象叫做 **事件对象**

- 浏览器给了我们一个 **黑盒子**，叫作 **window.event**，就是对事件信息的所有描述

  - 比如点击事件

  - 你点在了0，0位置，那么你得到的这个事件对象里面对应的就会有这个点位的属性

  - 你点在了10，10位置，那么你得到的这个事件对象里面对应的就会有这个点位的属性

  - 。。。

    ```js
    oDiv.onclick = function(){
    	console.log(window.event.x轴坐标点信息)
    	console.log(window.event.y轴坐标点信息)
    }
    ```

### 18.4鼠标事件

- clientX，clientY 距离浏览器可视窗口的左上角的坐标值

- pageX，pageY距离页面文档流的左上角的坐标值

- offsetX，offsetY 距离触发元素的左上角的坐标值

- Pointer-event:none ；穿透

  鼠标跟随：

  ```js
      box1.onmouseover = function () {
          this.firstElementChild.style.display = 'block'
          console.log(this.firstElementChild)
      }
      box1.onmouseout = function () {
          this.firstElementChild.style.display = 'none'
          console.log(this.firstElementChild)
      }
      box1.onmousemove = function (evt) {
          this.firstElementChild.style.top = evt.offsetY + 'px'
          this.firstElementChild.style.left = evt.offsetX + 'px'
      }
  ```

  鼠标拖拽：

  ```js
      box1.onmousedown = () => {
          document.onmousemove = (e) => {
              const box = document.querySelector('#box1')
              const height = parseInt(getComputedStyle(box).height)
              const width = parseInt(getComputedStyle(box).width)
              const clientH = document.documentElement.clientHeight
              const clientW = document.documentElement.clientWidth
  
              if (e.clientX >= width / 2 && e.clientX <= clientW - width / 2) {
                  box.style.left = e.clientX - height / 2 + 'px'
              }
              if (e.clientY >= height / 2 && e.clientY <= clientH - height / 2) {
                  box.style.top = e.clientY - width / 2 + 'px'
              }
          }
      }
      box1.onmouseup = () => {
          document.onmousemove = null
      }
  ```

  

### 18.5事件的传播（冒泡）

- 当元素触发一个事件的时候，其父元素也会触发相同的事件，父元素的父元素也会触发相同的事件

  标准的dom事件流：

  捕获：window=>document=>body=>outer

  目标：inner

  冒泡：outer=>body=>document=>window

  默认情况，只在冒泡触发

  按照dom2事件绑定，并进行配置  才能看到捕获的回调函数被触发

### 18.6阻止事件传播

- 阻止事件传播
  - Evt.stopPropagation()
  - ie浏览器 ：evt.cancelBubble = true

### 18.7阻止默认行为

- 阻止默认行为

  ```js
  //dom0   return false 阻止默认行为
  document.oncontextmenu = function(){
   console.log("xxx")
   return false
  }
  
  //dom2		
  document.addEventListener('contextmenu',function(){
    console.log('xxx')
    evt.preventDefault()
  })
  ```

### 18.8事件委托

- 就是把我要做的事情委托给别人做

- 因为我们的冒泡机制，点击子元素的时候，也会同步触发父元素的相同事件

- 所以我们就可以把子元素的事件委托给父元素来做

  

  事件触发

  - 点击子元素的时候不管子元素有没有点击事件，只要父元素有点击事件，那么就可以出发父元素的点击事件

  target

  - target这个属性是事件对象里的属性，表示你点击的目标
  - 当你触发点击事件的时候，你点击在哪个元素上，target就是哪个元素
  - 这个target也不兼容，在iE下要使用srcElement

事件委托的好处：减少多个函数的绑定的性能损耗，		

## 19.正则表达式（复杂类型）

- 字面量：var reg =/abc/

- 内置构造函数： var reg = new RegExp('abc')

- 元字符

  - \d ：1位数字（0-9）		注：digit（数字）
  - \D：1位非数字
  - \s ：1位空格（空格 缩进 换行） 		注：whitespace（空白字符）
  - \S：1位非空白
  - \w：1位字母（大小写），数字，下划线 		注：word character（单词字符）
  - \W：1位非字母（大小写），数字，下划线
  - .：任意内容（换行不算）
  - \：转义字符

- 边界符

  - ^：开头		例：/^\d/ 以数字开头

  - $：结尾边界        例：/\d$/ 以数字结尾

  - ^开头结尾$： 例：/^abc$/ 必须以abc为开头，以abc为结尾

    ​						 例：/^a\dc$/ 以a为开头，中间一位任意数字，以c为结尾

- 限定符

  - *：0～多次
  - +：1～多次
  - ?：0～1 （可选项目
  - {n}：指定次数        注：只能修饰前面一个字符
  - {n,}：>=n。          注：只能修饰前面一个字符
  - {n,m}：大于等于你n，小于等于5             注：只能修饰前面一个字符

- 特殊字符

  - ()：整体
  - |：或，只关注左右的整体，不是一个     例 ：/ab|bc/  包含ab或bc
  - [] ：代表1位.               例：/[abcdef]/ 包含其中一位即可
  - [a-zA-Z0-9_] 等价于 \w
  - [0-9] 等价于 \d
  - [^abc]：^在这里是取反，包含一位不是a或b或c的字母 

- 标识符

  - g：全局匹配（global）

  - i：不区分大小写匹配（case-insensitive）

    ```js
    const str = "This is an Example, example and EXAMPLE.";
    const pattern = /example/gi;
    
    const matches = str.match(pattern);
    console.log(matches); // 输出: ["Example", "example", "EXAMPLE"]
    ```

- 常用方法

  - test() ：检测字符串是否符合指定格式
  - exec()：捕获片段，截取符合格式的字符串片段，能截取第一个符合条件的片段

- 正则表达式的特性

  - 懒惰匹配：解决方案，使用全局标识符g
  - 贪婪匹配：解决方案：非贪婪模式，匹配格式后价?

- 正则与字符串方法

  - 正则.test(字符串)

  - 正则.exec(字符串)

  - 字符串.replace：方法用于在字符串中查找指定的子串，并将其替换为另一个字符串。它返回一个新的字符串，原始字符串不会被修改。

    ```js
    const sentence = "I have 3 apples and 2 oranges.";
    const pattern = /\d+/g; // 匹配一个或多个数字
    const replaced = sentence.replace(pattern, "some"); // 将数字替换为 "some"
    console.log(replaced); // 输出: "I have some apples and some oranges."
    ```

  - 字符串.search：方法用于在字符串中查找指定的子串，返回第一个匹配子串的索引。如果没有找到匹配项，则返回 -1。

    ```js
    const text = "The pattern is found at position 12.";
    const pattern = /pattern/;
    const index = text.search(pattern);
    console.log(index); // 输出: 4
    ```

  - 字符串.match：方法用于在字符串中查找与正则表达式匹配的子串，返回一个数组，其中包含找到的匹配项。如果没有找到匹配项，则返回 `null`。

    ```js
    const sentence = "The code is ABC123 and XYZ456.";
    const pattern = /[A-Z]{3}\d{3}/g; // 匹配三个大写字母后跟三个数字
    const matches = sentence.match(pattern);
    console.log(matches); // 输出: ["ABC123", "XYZ456"]
    ```

## 20.this关键字

- this 谁调用我，this就指向谁 （es6 箭头函数）

```
//全局
console.log(this)  //this：指向window

//对象
var obj = {
	name:'chris',
	test:function(){
		console.log('qqq',this)
	}
}
obj.test()  //this指向obj


//setTimeout
setTimeout(function(){console.log(this)},2000) //this：指向window

//事件绑定的this （dom0和dom2）
box节点
box.onclick = function(){
	console.log(this)            //this：指向box节点
}

```

- 改变this指向的方法

  - call / apply /  bind

    ```js
        const obj1 = {
            name: 'obj1',
            getName: function () {
                console.log('getName1:', this.name)
            }
        }
        const obj2 = {
            name: 'obj2',
            getName: function () {
                console.log('getName2:', this.name)
            }
        }
    
        obj1.getName()
        obj2.getName()
    
        //call 执行函数，并改变this执行为行数的第一个参数
        //支持多个参数
        obj1.getName.apply(obj2)
    
        //apply 执行函数，并改变this执行为行数的第一个参数
        //支持2个参数，第二参数是一个数组
        obj1.getName.apply(obj2)
    
        //bind 改变this指向为函数的第一个单数，不会自动指向函数
    	 //支持多个参数
        const fun1 = obj1.getName.bind(obj2)
        console.log(fun1)
        fun1()
    ```

## 21.ES6

- let ，const（与var的区别）

  - 必须先定义在使用
  - 变量不能重名
  - 块级作用域，let只存活在块中

- let，const的区别

  - let定义变量
  - const定义常量

- 箭头函数：(参数)=>{返回值或方法体}

  - 只有一个形参的时候，()可以省略
  - {}可以省略，只有一句代码，只有返回值的时候
  - 没有arguements（不能用argeuments，接收形参）
  - 箭头函数没有this，箭头函数this是父级作用域

- 函数的默认参数

  - function test(a=1,b=2){return a+b}
  - const test =(a=1,b=2)=>a+b

- 解构赋值

  - 快速地从对象和数组中获取里面的成员

  - 对象结构可以重命名

    ```js
    var obj= {
    	name:'chirs',
    	age:11,
    	location:'yokohama'
    }
    //age重命名为myage  /  location重命名为mylocation
    let {name,age:myage,location:mylocation} =obj
    
    //也可以多维对象解构
    ```

  - 对象简写

    ```js
    let username = 'chirs'
    let password = '12345'
    //属性名和变量名相同时，可以简写
    var obj ={	
    	username,    //username:username
    	password		 //password:password
      getName(){}  //getName:function(){}
    }
    ```

- 展开运算符 ...

  - 展开数组

    ```js
    let a = [1,2,3]
    let b = [4,5,6]
    var c = [...a,...b]
    console.log(c)  //[1,2,3,4,5,6]
    ```

  - 复制

    ```js
    let a = [1,2,3]
    let b = [...a]
    ```

    - ...参数-实参-形参     **注意：必须放在最后一个参数**

      ```js
      //形参
      let test = function(a,b,...arr){
        console.log(arr)
      }
      test(1,2,3,4,5)  //[ 3, 4, 5]
      
      //实参
      let arr = [1,2,3]
      const test1 = function(a,b,c){
        console.log(a,b,c)
      }
      test1(...arr)   //1,2,3
      ```

  - ...伪数组转换

    ```js
    function test(){
    	var arr = [...arguments]
    	console.log(arr)
    }
    test(1,2,3,4,5)  //[1, 2, 3, 4, 5]
    ```

  -  ...对象   **注意：同名属性会被覆盖**

    ```js
    const obj1 = {name:'chris',age:100}
    const obj2 = {location:'yokohama'}
    
    let obj = {
      ...obj1,
      ...obj2
    }
    console.log(obj)      //{name: 'chris', age: 100, location: 'yokohama'}
    ```

- 模块化

  - 私密不漏
  - 重名不怕
  - 依赖不乱

## 22.面向对象

- 自定义构造函数

  - 函数名首字母大写

  - 构造函数不写return

  - this指向，谁调用指向谁

    ```js
    function CreateObj(name){
    	this.name = name
    }
    var obj1 = new CreateObj('chris') //new过程===实例化这个过程
    console.log(obj1)  //CreateObj {name: 'chris'}
    ```

- `对象.__proto__===构造函数.prototype`

- 原型链概念

- class（ES6-语法糖）

  ```js
  class Person{
    //构造函数
      constructor(name,age) {
          this.name = name
          this.age = age
      }
      sayHi(){
          console.log('I am '+this.name+', '+this.age+' years old!')
      }
  }
  
  let chris = new Person('chris',22)
  chris.sayHi()
  ```

- 面向对象的继承

  - 构造函数继承---继承属性

    ```js
    function Student(name,age,classroom){
      Person.call(this,name,age)
      this.calssroom =calssroom
    }
    ```

  - 原型继承---继承原型上方法

    ```js
    // 父类构造函数
    function Animal(name) {
      this.name = name;
    }
    
    // 父类方法
    Animal.prototype.sayHello = function() {
      console.log(`Hello, I'm ${this.name}`);
    };
    
    // 子类构造函数
    function Dog(name, breed) {
      // 调用父类构造函数
      Animal.call(this, name);
      this.breed = breed;
    }
    
    // 子类继承父类的原型
    Dog.prototype = Object.create(Animal.prototype);
    
    // 子类方法
    Dog.prototype.bark = function() {
      console.log(`${this.name} is barking`);
    };
    
    // 创建子类实例
    const myDog = new Dog('Buddy', 'Golden Retriever');
    
    // 调用继承的方法
    myDog.sayHello(); // 输出: Hello, I'm Buddy
    
    // 调用子类方法
    myDog.bark(); // 输出: Buddy is barking
    ```

  - 组合继承

    构造函数继承+原型继承

  - ES6继承

    ```js
    // 父类定义
    class Animal {
      constructor(name) {
        this.name = name;
      }
    
      sayHello() {
        console.log(`Hello, I'm ${this.name}`);
      }
    }
    
    // 子类继承父类
    class Dog extends Animal {
      constructor(name, breed) {
        super(name); // 调用父类构造函数
        this.breed = breed;
      }
    
      bark() {
        console.log(`${this.name} is barking`);
      }
    }
    
    // 创建子类实例
    const myDog = new Dog('Buddy', 'Golden Retriever');
    
    // 调用继承的方法
    myDog.sayHello(); // 输出: Hello, I'm Buddy
    
    // 调用子类方法
    myDog.bark(); // 输出: Buddy is barking
    ```

## 23.Ajax（async javascript and xml）

1. **Ajax（Asynchronous JavaScript and XML）**：
   - Ajax 是一种用于创建异步网络请求的技术，通过浏览器内置的 `XMLHttpRequest` 对象实现。
   - Ajax 可以在不刷新整个页面的情况下，通过后台请求数据并将数据呈现在页面上。
   - 使用 Ajax 需要手动编写代码来处理异步请求和响应。
2. **XMLHttpRequest**：
   - `XMLHttpRequest` 是浏览器提供的原生对象，用于创建异步网络请求。
   - 它是 Ajax 技术的基础，可以手动设置请求头、发送数据和监听响应。
   - 使用 `XMLHttpRequest` 需要编写相对繁琐的代码，需要处理不同浏览器之间的兼容性问题。
3. **Promise**：
   - Promise 是一种用于处理异步操作的模式，它可以更优雅地处理回调地狱问题，提供了更清晰的异步代码结构。
   - Promise 对象代表一个异步操作的最终完成或失败，并可以通过 `.then()` 和 `.catch()` 方法来处理结果。
   - 使用 Promise 可以更方便地编写异步代码，但仍然需要使用原生的 `XMLHttpRequest` 或其他网络请求库。
4. **Fetch**：
   - `fetch()` 是浏览器内置的现代网络请求 API，用于发起网络请求并返回 Promise。
   - 它提供了更简洁的 API，支持链式调用和异步操作处理，但在处理错误和请求细节方面相对较少的控制。
   - `fetch()` API 相对新，可能需要考虑浏览器兼容性。
5. **Axios**：
   - Axios 是一个流行的第三方网络请求库，用于在浏览器和 Node.js 中发起请求。
   - 它基于 Promise，提供了更丰富的功能，如拦截请求、响应转换、取消请求等。
   - Axios 具有更好的浏览器兼容性，使得处理网络请求变得更加方便。

联系和区别：

- Ajax 是一种通用的异步请求技术，而 `XMLHttpRequest` 是其基础。
- Promise 是一种用于处理异步操作的模式，可以与 Ajax、`XMLHttpRequest` 以及其他网络请求工具一起使用。
- Fetch 是现代浏览器内置的网络请求 API，Axios 则是第三方库，都可以用来发起网络请求，但 Axios 提供了更多的功能和控制。

## 24.cookie

- 只能存储文本

-  单条存储拥有大小限制4KB左右

  数量限制（一般浏览器，限制大概在50条左右）

- 读取有域名限制；不可跨域读取，只能由来自写入cookie的同一域名的网页可进行读取。简单地讲就是，哪个服务器发给你的cookie，只有哪个服务器有权利读取

- 时效限制：每个cookie都有时效，默认的有效期是，会话界别：就是当浏览器关闭，那么cookie立即销毁，但是我们也可以存储的时候手动设置cookie的过期事件

- 路径现实；存cookie的时候可以指定路径，只允许子路径读取外层cookie，外层不能读取内层

## 25.jsonp

Jsonp（JSON with Padding）是jso的一种“使用模式”，可以让网页从别的域名（网站）获取资料，即跨域读取数据。

为什么我们从不同的域（网站）访问数据需要一个特殊的技术（JSONP）呢？这是应为同源策略。

**同源策略：同协议，同域名，同端口号**

**不符合同源策略，浏览器为了安全，会阻止请求**

解决：

1.cors:    由后端设置，Access-Control-Allow-Origin:*

2.jsonp:  前后端共同努力 

- jsop原理：动态创建Script标签，src 属性指向没有跨域限制，

​						指向一个接口，接口返回的格式一定是`****（）`函数表达式

- jsonp缺点
  - onload删除script标签
  - 只能get请求，不能post / put / delete

3.反向代理

## 26.闭包

1.记住列表的索引

2.jsqonp案例优化--函数防抖
