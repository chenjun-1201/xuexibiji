[toc]



# js基础篇



## 五大基本数据类型



- String  
- number
- boolean
- underfind
- Null
- 数据类型object不是基本数据类型



## 

**运算符 typeof**

```js
typeof "Bill"                 // 返回 "string"
typeof 3.14                   // 返回 "number"
typeof NaN                    // 返回 "number"
typeof false                  // 返回 "boolean"
typeof [1,2,3,4]              // 返回 "object"
typeof {name:'Bill', age:62}  // 返回 "object"
typeof new Date()             // 返回 "object"
typeof function () {}         // 返回 "function"
typeof myCar                  // 返回 "undefined" *
typeof null                   // 返回 "object"
```





### 强制类型转换

## 把数值转换为字符串

- 全局方法 String() 能够把数字,boolean,转换为字符串。
- x.toString()
- 把字符串转换为数值Number()

```js

alert( Number("333 888")); //返回NaN  （not a number）
```



- 自动字符串转换

```
document.getElementById("demo").innerHTML = myVar;

// 如果 myVar = {name:"Fjohn"}  // toString 转换为 "[object Object]"
// 如果 myVar = [1,2,3,4]       // toString 转换为 "1,2,3,4"
// 如果 myVar = new Date()      // toString 转换为 "Fri Aug 07 2020 19:32:34 GMT+0800 (CST)"
```



## 运算符



- 一元运算符 +取正  -取负
- 关系，逻辑，。。。运算符（省略）







## 对象

**重点**

```js
//三种创建对象的方式
var obj=new object();
obj.gender="hello";
obj.hhh="sdfsf";
alert(obj)


var obj2={};
obj2.name="zhangsan";
alert(obj2);


var obj3={
  name:"hello",
  age:89
  
}
alert(obj3);
```



基本数据类型保存在栈内存中，引用数据类型，值保存在堆内存，变量保存了该值得地址，通过这个地址对堆内存之间的数据进行引用。



## **函数定义 **



````js
var a="hello";
function a(a){
  alert(a);
 // return underfind;
}

alert(a()); //结果return underfind;

//提前式声明
//像function a(a){}这种样子声明的函数可以在函数声明前调用,不会报错
b();

function b(){
  alert("hello world");
 // return underfind;
}

//如下不是提前式声明
c();//控制台报错
function c=function(){
  alert("hello world");
 // return underfind;
}

//匿名函数,没有方法名，


function(){
  alert("hello world");
  return 999;
 // return underfind;
}


//匿名函数立即执行
(function(a,b){
  alert("hello world");
  alert(a+b);
 // return underfind;
})("hello","world");//如果函数有参数，则后面的括号里面可以加参数

````



构造函数

```js
//规则，构造函数也是一个普通函数，跟普通函数没有什么区别，只是构造函数的首字母是大写的
//2使用方式不一样，构造函数通过new关键字调用
function Person(name,age){
  this.name=name;
  this.age=age;
  
}
var v=new Person("zhangsan",28);

alert(v instanceof Person);
//可以使用实例 instanceof 类名判断是否是该类的实例


```





## 数组

常用方法

- slice(start ,end)  返回指定位置的数组

```js

<script>
var a=[1,2,3,4,5,6];
var b= a.slice(0,3);
alert(b);

</script>
```



- splice(int, count,c,d)  删除元素

```js

var fruits = ["Banana", "Orange", "Apple", "Mango"];


document.getElementById("demo1").innerHTML=fruits.splice(2,2,"wiki","vivo").join("*");
alert(fruits);
//返回删除掉的元素，从第二个起后面可以加任意数量的参数，参数代表网删除的地方新增加元素，这个方法可以用于给数组切入任意数量的元素
```

- join() 方法也可将所有数组元素结合为一个字符串。

```js
<script>
var fruits = ["Banana", "Orange", "Apple", "Mango"];
document.getElementById("demo").innerHTML = fruits.join(" * ");//该方法不会对原数组产生影响
</script>
```

- concat
- sort
- reverse ：将数组进行颠倒
- sort





## 	debug





## 函数

- call
- apply
- arguments



## Date



````js
var obj=new Date();
alert(obj);
//创建一个日期
var date2=new Date("11/09/2016 11:11:09");
alert(date2);
````



- getDay
- getMonth
- getDate
- getTime//获取当前时间的时间戳



## Math

Math是一个工具类

- PI ：圆周率常量，大写的属性都是常量
- floor()
- ceil()
- round()
- abs()
- random()



## String

- charAt()
- concat():连接字符串
- indexOf:查找指定字符

```js

var jj="hello hpoiuer";
alert(jj.indexOf("i"));

```

- lastIndexOf()
- slice(start ,end):

start:要抽取的片断的起始下标。如果是负数，则该参数规定的是从字符串的尾部开始算起的位置。也就是说，-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推



- split()

```js
//split可以根据正则表达式去拆
var vjj="a1b2c3d4e5";
 var u=vjj.split(/[a-z]/);
alert(u);
```



- toUpperCase
- toLowerCase
- match() 匹配正则表达式

```js
reg=/[a-z]/gi  //g是全局匹配，默认只返回查到的第一条，i可以忽略大小写
var vjj="a1b2c3D4E5";
var uv=vjj.match(reg);
alert(uv);//返回的结果是一个数组
alert();
 var u=vjj.split(/s/);


```



- subString()

```js
//创建正则表达式的对象RegExp
//RegExp中的方法 test(),compile(),	exec()
//[]用于查找某个范围内的字符：
reg=/a|b/;//等价于[ab]
reg.test("sb");
//[^ab]除了a和b
var b=new RegExp("s",/a/);
b.test("ksldjfowiejofiw")
```



- replace()
- search()

```js
//只会查找第一个
```





## 正则表达式

[正则表达式](https://www.w3school.com.cn/jsref/jsref_obj_regexp.asp)



````js
var reg=/aaa/  //等价于 reg=/a{3}/
var reg=/(ab){3}/  //等价于/ababab/

//a{x}代表a连续出现x次
//{m,n}出现m到n次
//{m,}出现m次以上

reg=/^a$/ //必须完全匹配
reg=/^a|a$/  //以a开头或以a结尾
/a/
  
  //创建一个表达式，判断他是否是一个手机号
  
  reg=/^1[3-9][0-9]{8}$/

reg=/\\/ //匹配一个/
reg=/\./  //匹配.


//\b单词边界
str=" child  ";
reg=/child/;
reg.test(str);
reg=/\dchild\b/;
reg.test(str);
//\B除了单词边界

//去除字符串前面的空格
str="     he  llo  ";
str=str.replace(/^\s+/g,"");//去除前面的空格
console.log(str);

//reg=/\s$/g以空格结尾
str=str.replace(/^\s+|\s+$/g/,"kdsjfk");//去除两端空格
alert(str+0);
console.log("hello");


````





```js
<html>
<body>
<h1>注册
<input>用户名<p id="demo1"></p><br> 
<input>密码<p id="demo2"></p><br>
<input>邮箱<p id="demo3"></p><br>
//<input>用户名<p id="demo4"></p><br>

<script type="text/javascript">
var str1="用户名格式不对用户名不能为纯数字";
var str2="密码格式或数量不对";
var str3="邮箱格式不对";
var str4="密码格式或数量不对";
reg=/sdf/;

/*
邮箱首字符和末尾字符必须为字母或数字，邮箱名可以全是字母或数字，或者是两者的组合；
连字符"-"、下划线"_" 和英文句号点"."，仅能放在字母或数字中间，且不能连续出现（即其单个符号的左右只能是字母或数字）；
域名可以带连字符"-"， 且可以是多级域名 ,还可以有多个域名后缀；
不区分大小写；
不限定邮箱字符串的具体长度。*/
youxiang=/^[a-Z]|[0-9]/
</script>

</body>
</html>


```

## Dom

是Document  Object model 的简写

通过将html文档 的每一个节点（node）转换为对象，在对对象进行操作。

node：

- 文档节点 ：  Document 对象是 Window 对象的一部分，可通过 window.document 属性对其进行访问。
- 元素节点 ：html标签
- 属性节点： 标签中的属性
- 文本节点  : 标签中的节点



```js
//js提供了一个对象用于操作文档:document
alert(document);
<p id="demo">jj</p>
var h= document.getElementbyid("demo");
h.innerHTML="helloworld";

```

​		

## 事件

给对象添加事件

```html
//可以通过在标签中加onclick
<button onclick="aa()"></button>

HTML 4.0 的新特性之一是能够使 HTML 事件触发浏览器中的行为，比如当用户点击某个 HTML 元素时启动一段 JavaScript。下面是一个属性列表，可将之插入 HTML 标签以定义事件的行为。

onabort	图像的加载被中断。
onblur	元素失去焦点。
onchange	域的内容被改变。
onclick	当用户点击某个对象时调用的事件句柄。
ondblclick	当用户双击某个对象时调用的事件句柄。
onerror	在加载文档或图像时发生错误。
onfocus	元素获得焦点。
onkeydown	某个键盘按键被按下。
onkeypress	某个键盘按键被按下并松开。
onkeyup	某个键盘按键被松开。
onload	一张页面或一幅图像完成加载。
onmousedown	鼠标按钮被按下。
onmousemove	鼠标被移动。
onmouseout	鼠标从某元素移开。
onmouseover	鼠标移到某元素之上。
onmouseup	鼠标按键被松开。
onreset	重置按钮被点击。
onresize	窗口或框架被重新调整大小。
onselect	文本被选中。
onsubmit	确认按钮被点击。
onunload	用户退出页面。

相当于标签的一个属性


```

Document中的事件

```html
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript 数组方法</h1> 

<h2>splice()</h2>

<p>splice() 方法可用于删除数组元素。</p>
	
<button onclick="myFunction()">试一试</button>
<input type="radio"  value="羽毛球"/>羽毛球
<input type="radio" value="乒乓" />乒乓
<input type="radio" value="网球"/>网球

<p id="demo"></p>

<script>
window.onload=function ss(){

var btn= document.getElementsByTagName("button")[0];
var list= document.getElementsByTagName("input");

btn.onclick=function myFunction(){

alert(list[1]);
}

alert(1);
function myFunction(){
alert(list[1]);
};
</script>

  //总结:元素中的任意的属性赋值都可以通过doucment获取该标签.属性值=值
  去改变元素的任意属性
</body>
</html>


```





文档加载：默认情况下是自上而下，如果将js代码写到加载标签的前面则会报错

解决办法：window.onload() 等window加载完成后进行调用。

onload 事件会在页面或图像加载完成后立即发生。

 onload支持绑定的对象 image, layer, window

```js
window.onload()=function jj(){//该方法会等页面加载完成之后才执行
  alert("jj");
}

```





获取元素的子节点

- getElementbyname();
- childnotes()
- firstchild()
- lastchild()

获取父节点

- parentNode
- previousibling
- next sibling





## document修改css样式



document.get元素.style.样式=样式值

可以去w3school参考文档找css属性修改的方法

```js
object.style.backgroundColor="#00FF00"
```



## 事件对象

```js
//当事件方法被调用时，浏览器会传一个事件对象到方法中，该对象就是刚发生的事件
//
//Event 对象


		<div id="bigdiv" onmousemove="jj" style="width: 200px; height: 300px; background-color: aqua;" >
  
			<div id="smalldiv" style="width: 100px; height: 30px; background-color: bisque;">

				
  window.onload=function(){
        
       
				var x;
				var y;
			var i=document.getElementById("bigdiv");
			var o=document.getElementById("smalldiv");
			i.onmousemove=function(event){//event为形参，真正的参数是由浏览器传入，event接收浏览器的事件对象
           alert(event);//结果为一个事件对象MouseEvent

        //事件对象的属性clientX返回鼠标指针的水平坐标。
				x=event.clientX;
				y=event.clientY;
				o.innerHTML="x="+x+"y="+y;
        //注意IE8不支持event，支持window.event

			}
			

};
  
```

事件对象常用的属性

- clientX,clientY
- pageX,pageY



## 事件的冒泡



事件的委派

事件的绑定

- addEventListener绑定鼠标点击事件

```js
img.addEventListener("click",
        function(){
        alert("sdfsdf");
        }//IE8不支持
    );
img.addEventListener("click",
        function(){
        alert("sdfsdf");
        }//IE8不支持
    );
//该方法可以绑定多个事件，于onclick最大的区别，注意参数“click”不是"onclick"
```







## 鼠标拖拽

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>鼠标拖拽事件</title>
    <style type="text/css">
    img{
        position: absolute;
    }
    </style>

    <script>
        window.onload=function(){
           
                
    var img= document.getElementsByTagName("img")[0];

    img.onmousedown=function(event){
      //div的偏移量 鼠标.clentX - 元素.offsetLeft
        //div的偏移量 鼠标.clentY - 元素.offsetTop
        
    var  ol=event.clientX-img.offsetLeft;
    var  ll=event.clientY-img.offsetTop;

    document.onmousemove=function(event){

         //减去偏移量，使鼠标相对于图片的位置不变
        img.style.left=event.clientX-ol+"px";
        img.style.top=event.clientY-ll+"px";

    }
}

img.onmouseup=function(){
    //拖拽功能取消
document.onmousemove=null;

//取消，这个方法调用完就要取消，不用
img.onmouseup=null;
    
}
        }


    </script>
</head>
<body>
    <img src="https://www.w3school.com.cn/i/eg_mouse2.jpg">
    

    
</body>
</html>
```

- onmousemove
- onmousedown
- onmouseup
- 鼠标拖拽用到的三个事件



## 鼠标滚轮事件

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			
			#box1{
				width: 100px;
				height: 100px;
				background-color: red;
			}
			
		</style>
		<script type="text/javascript">
			
			window.onload = function(){
				
				
				//获取id为box1的div
				var box1 = document.getElementById("box1");
				
				//为box1绑定一个鼠标滚轮滚动的事件
				/*
				 * onmousewheel鼠标滚轮滚动的事件，会在滚轮滚动时触发，
				 * 	但是火狐不支持该属性
				 * 
				 * 在火狐中需要使用 DOMMouseScroll 来绑定滚动事件
				 * 	注意该事件需要通过addEventListener()函数来绑定
				 */
				
				
				box1.onmousewheel = function(event){
					
					event = event || window.event;
					
					
					//event.wheelDelta 可以获取鼠标滚轮滚动的方向
					//向上滚 120   向下滚 -120
					//wheelDelta这个值我们不看大小，只看正负
					
					//alert(event.wheelDelta);
					
					//wheelDelta这个属性火狐中不支持
					//在火狐中使用event.detail来获取滚动的方向
					//向上滚 -3  向下滚 3
					//alert(event.detail);
					
					
					/*
					 * 当鼠标滚轮向下滚动时，box1变长
					 * 	当滚轮向上滚动时，box1变短
					 */
					//判断鼠标滚轮滚动的方向
					if(event.wheelDelta > 0 || event.detail < 0){
						//向上滚，box1变短
						box1.style.height = box1.clientHeight - 10 + "px";
						
					}else{
						//向下滚，box1变长
						box1.style.height = box1.clientHeight + 10 + "px";
					}
					
					/*
					 * 使用addEventListener()方法绑定响应函数，取消默认行为时不能使用return false
					 * 需要使用event来取消默认行为event.preventDefault();
					 * 但是IE8不支持event.preventDefault();这个玩意，如果直接调用会报错
					 */
					event.preventDefault && event.preventDefault();
					
					
					/*
					 * 当滚轮滚动时，如果浏览器有滚动条，滚动条会随之滚动，
					 * 这是浏览器的默认行为，如果不希望发生，则可以取消默认行为
					 */
					return false;
					
					
					
					
				};
				
				//为火狐绑定滚轮事件
				bind(box1,"DOMMouseScroll",box1.onmousewheel);
				
				
			};
			
			
			function bind(obj , eventStr , callback){
				if(obj.addEventListener){
					//大部分浏览器兼容的方式
					obj.addEventListener(eventStr , callback , false);
				}else{
					/*
					 * this是谁由调用方式决定
					 * callback.call(obj)
					 */
					//IE8及以下
					obj.attachEvent("on"+eventStr , function(){
						//在匿名函数中调用回调函数
						callback.call(obj);
					});
				}
			}
			
		</script>
	</head>
	<body style="height: 2000px;">
		
		<div id="box1"></div>
		
	</body>
</html>

```





## 键盘事件

- onkeydown

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<style type="text/css">
    #demo1 {
        /*
        
        */
        width: 130px;
        height: 130px;


        /*
         * 开启box1的绝对定位
         */
        position: absolute;
    }
</style>


<body style="width: 1000px ; height: 2000px;">

    <!-- <img src="./13a17.jpg" id="demo1" onmousemove=""> -->

    <div style="left: 300;"><input id="demo2"></div>
    <a href="www.baidu.com">前往百度</a>
    <script>

        window.onload = function () {

            var input = document.getElementById("demo2");

          /*
				 * 键盘事件：
				 * 	onkeydown
				 * 		- 按键被按下
				 * 		- 对于onkeydown来说如果一直按着某个按键不松手，则事件会一直触发
				 * 		- 当onkeydown连续触发时，第一次和第二次之间会间隔稍微长一点，其他的会非常的快
				 * 			这种设计是为了防止误操作的发生。
				 * 	onkeyup
				 * 		- 按键被松开
				 * 
				 *  键盘事件一般都会绑定给一些可以获取到焦点的对象或者是document
				 */
            input.onkeydown = function (event) {

                // console.log(event.keyCode);
                if (event.keyCode === 89 && event.shiftKey) {//判断y键和shift键是否同时按下
                    alert("同时按下了y和shift");
                }

                    //判断keycode，如果输入的是中文，取消默认行为
                if (event.keyCode >= 48 && event.keyCode <= 57) {

                    return false;//取消默认行为

                }
            }

            document.getElementsByTagName("a")[0].onclick = function () {

                return false;
            }


        }
    </script>
</body>

</html>
```



- onkeyup