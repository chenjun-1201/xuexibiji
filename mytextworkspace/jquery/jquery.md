# jquery

## 选择器

- 基本选择器
- 层次选择器
  - 过滤选择器  ：${ul li:eq(2)}  ${ul li:odd}  奇数行 ${ul li:even}  偶数行
  - 筛选方法



隐式迭代：

jquery帮我们迭代好了，我们得到的list dom对象会自动遍历



```js
$("button").addClass("animated bounce");
$(".well").addClass("animated shake");
$("#target3").addClass("animated fadeOut");
```



/usr/local/bin/python3 

jquery的方法：

- addClass("参数名1 参数名2")
- removeClass() 删除
- $("#target1").css("color", "red");
- $("#target1").prop("disabled",true);
- html("<em>#target4</em>")    :修改文本内容 可以读取标签名
- remove() 删除一个元素
- `appendTo()`方法  可以让你把选中的HTML元素附加到其他元素中。
- clone方法：  $("#target5").clone().appendTo("#left-well");
-  parent（）                     $("#target1").parent().css("background","blue");
-  children()                $("#right-well").children().css("color","orange")
-  $(".target:nth-child(2)").addClass("bounce");
- .eq()   也可以写成$("li:eq()")
- siblings("li")  获取所有兄弟元素
- .index()  获取当前元素的下标，  $("li").index();



初始化方法

```js
$(document).ready({

});
```



