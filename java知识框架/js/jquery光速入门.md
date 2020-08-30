# jquery光速入门

 jquery的优势

- 浏览器兼容
- 更方便，更简洁、
- api简单，容易记忆



jquery 就是一个封装了很多js代码的库



## 入口函数

入口函数与window.onload的区别：

- 入口函数可以有多个，window.onload只能有一个，再写就会被覆盖
- 入口函数执行速度优于js，加载Dom就能执行，window.onload要等所有的资源（.css）加载完后才执行



jquery对象

使用jquery选择器找出来的对象

```js
//两种方式使用入口函数
$(function{
  //内容代码
  });

//2
$(document).ready(function(){
  
   //内容代码
}

);
```



## jquery实例

这些方法比较常用，可以去w3shool去多看多敲

- $("test").hide()
- $("test").html()
- $("test").click()
- hover(),mouser leave()

```js
 $(document).ready(function(){ 
$("div:eq(2)").hover(//有两个参数方法，一个是鼠标移入，一个是鼠标移出
            function jin() {
                $(this).html("123");
            },
            function chu() {
                $(this).html("888");
            }

        )

    });

```



- 





## jquery选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <script></script>
<script src="js/jquery/jquery-3.4.1.js"></script>
</head>
<body>

<p>

</p>

<div id="demo">阿童木
<p>喜洋洋</p>
</div><br>
<div id="demo2">
  <p>
    
  <span></span>
    <span>米老鼠</span>
    <span></span>

</p></div><br>
  
<div>变形金刚</div><br>

<div>lsdkj</div><br>
<script>

    $(document).ready(function(){

        alert($("div:eq(2)").html())  ;//获取第三个div元素   eq代表equals

        alert($("#demo").html());//使用id选择器

        alert($("#demo p:eq(0)").html());//获取div里面的第一个p标签中的值

        alert($("#demo2 p:eq(0) span:eq(1)").html());  //使用更加复杂一点的选择器，
    });


       // $(function () {
       //     alert("hello");
       // });





</script>

</body>
</html>

```

