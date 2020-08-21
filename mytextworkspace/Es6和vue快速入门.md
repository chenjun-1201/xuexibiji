# Es6和vue快速入门

## vue模块化开发：



```shell


全局安装webpack

npm install webpack -g

全局安装vue cli

 npm install -g @vue/cli-init

使用cli-init快速创建脚手架工程

在项目中使用：

1先创建一个文件夹vuedemo

cd ......省略.... vuedemo(定位创建项目的位置)

使用vue init  webpack vuedemo

然后配置相关配置。

会在vuedemo生成一个vuedemo的项目

运行Vue run dev 启动
```



安装element ui 一个Vue的组件库

```shell
npm i element-ui
```



## vue语法：

### 绑定 ###

#### 1文本插值

注意如果没有效果就修改vue.js的路径

数据绑定最常见的形式就是使用 {{...}}（双大括号）的文本插值

```html
<div id="app">
  <p>{{ message }}</p>
</div>
```



#### 1v-bind:

**HTML 属性中的值应使用 v-bind 指令。**

```html
<body>
    <div id="haha">

        <a v-bind:href="link">hah</a>
        <!-- 使用v-bind还可以绑定style -->
    <span id="jj" v-bind:style="{color:c1,fontSize:f1}">你好</span>
    <input >
</div>
<!-- 引入vue.js -->
    <script src="../vue2/node_modules/vue/dist/vue.js"></script>
<!-- 使用v-bind绑定a标签的属性 -->
    <script>
        let vue=new Vue({
            el:"#haha",
            data:{
                link:'http://www.baidu.com',
                c1:"red",
                f1:'80px'
            }
        });
    </script>


    <!--  -->

</body>
```



#### v-model双向绑定

双向绑定：vue绑定的元素的值会随控件input,checkbox的变化而变化，同理，input,checkbox的值也会随vue绑定的元素的值的变化而变化

```html
<!-- 使用v-model对input和checkbox进行双向绑定 -->

 <input v-model="num" >


  <div id="app">

        精通的语言：
            <input type="checkbox" v-model="language" value="Java"> java<br/>
            <input type="checkbox" v-model="language" value="PHP"> PHP<br/>
            <input type="checkbox" v-model="language" value="Python"> Python<br/>
        选中了 {{language.join(",")}}
    </div>


<script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        let vm = new Vue({
            el:"#app",
            data:{
                language: []
            }
        })
    </script>
```



### 计算

#### computed

监听：watch

```html
<div id="app">
        <!-- 某些结果是基于之前数据实时计算出来的，我们可以利用计算属性。来完成 -->
        <ul>
            <li>西游记； 价格：{{xyjPrice}}，数量：<input type="number" v-model="xyjNum"> </li>
            <li>水浒传； 价格：{{shzPrice}}，数量：<input type="number" v-model="shzNum"> </li>
            <li>总价：{{totalPrice}}</li>
            {{msg}}
        </ul>
    </div>

    <script src="../vue2/node_modules/vue/dist/vue.js"></script>

    <script>

 let vm= new Vue({
            el: "#app",
            data: {
                xyjPrice: 99.98,
                shzPrice: 98.00,
                xyjNum: 1,
                shzNum: 1,
                msg: ""
            }

            // 使用后只要这些值有变化，都会自动调用这个方法
          ,  computed: {
                totalPrice(){
                    return this.xyjPrice*this.xyjNum + this.shzPrice*this.shzNum
                }
            }

            ,
            //监听器
            watch: {
                xyjNum(var1,var2){
                        //方法名必须为绑定的属性名，属性一旦变化就会调用该方法
                    alert("hhhh");

                }
            },
        });

       
    </script>
```



 过滤器

:question:

**filters**



:airplane:

### 组件化



上代码：

````js

//全局生成模板：
Vue.component("hello",{

template:" <button v-on:click=count++>我被点击了{{count}}次~~~</button>",
data(){

    return {
        count:1
    };
}


});

new Vue({

            el:"#app",
            data:{

                count:1
            }
            
        });
````

然后在html body中使用<hello></hello>即可使用该模板



局部生成模板

局部生成的模板调用时要在每个vue实例中引入

```js
/局部生成组件

const buttoncount={

template:" <button v-on:click=count++>我被点击了{{count}}次~~~++++</button>",
data(){

    return {
        count:1
    };
}


}
;

new Vue({

            el:"#app",
            data:{

                count:1
            }
,
            components:{
                "button_count":buttoncount
            }
            
        });
```



