# Bootstrap快速入门

1引入bootsrap 

````html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap /3.3.1/css/bootstrap.min.css"/>
````



2

```html
<img src="/statics/codecamp/images/running-cat.jpg" class="img-responsive">


```

```html
  <h2 class="text-center">
    jjj
  </h2>
```



```html
<button class="btn">
  Like
  </button>
```

​	

使button变成block元素

```html
  <button class="btn btn-block">
    hello world
  </button>

加个颜色btn-primary

<button class="btn btn-block btn-primary">Like</button>

info样式
 <button class="btn btn-block btn-info">
    Info
  </button>

删除
  <button class="btn btn-block btn-danger">
    Delete
  </button>



<div class="row">
<div class="col-xs-4">
<button class="btn btn-block btn-primary">Like</button>
</div>
<div class="col-xs-4">
<button class="btn btn-block btn-info">Info</button>
</div>
<div class="col-xs-4">
<button class="btn btn-block btn-danger">Delete</button>
</div>
</div>
```



### 使用Bootstrap创建一个头部标题

```html
<h3 class="text-center text-primary">
  jQuery Playground
</h3>
```



为了让 Bootstrap 开发的网站对移动设备友好，确保适当的绘制和触屏缩放，需要在网页的 head 之中添加 viewport meta 标签，如下所示：

````html
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">


所有html元素包括到这个元素中，这个是响应式布局屏幕100%
<div class="container-fluid"></div>
````





创建一个头部导航栏：

```html
<div class="row">
<div class="col-xs-8">
<h2 class="text-primary text-center">CatPhotoApp</h2>
</div>
<div class="col-xs-4">
<a href="#"><img class="img-responsive thick-green-border" src="https://www.w3cschool.cn/statics/codecamp/images/relaxing-cat.jpg"></a>
</div>
</div>
```





```html
<link href="//fonts.googleapis.com/css?family=Lobster" rel="stylesheet" type="text/css">

//引入图标
```

