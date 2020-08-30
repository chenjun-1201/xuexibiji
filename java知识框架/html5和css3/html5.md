# html5

## h5新特性

- 音频

````html
<audio src="song.ogg" controls="controls">
</audio>
````

- 视频

  - | 属性                                                         | 值       | 描述                                                         |
    | :----------------------------------------------------------- | :------- | :----------------------------------------------------------- |
    | [autoplay](https://www.w3school.com.cn/tags/att_video_autoplay.asp) | autoplay | 如果出现该属性，则视频在就绪后马上播放。                     |
    | [controls](https://www.w3school.com.cn/tags/att_video_controls.asp) | controls | 如果出现该属性，则向用户显示控件，比如播放按钮。             |
    | [height](https://www.w3school.com.cn/tags/att_video_height.asp) | *pixels* | 设置视频播放器的高度。                                       |
    | [loop](https://www.w3school.com.cn/tags/att_video_loop.asp)  | loop     | 如果出现该属性，则当媒介文件完成播放后再次开始播放。         |
    | [muted](https://www.w3school.com.cn/tags/att_video_muted.asp) | muted    | 规定视频的音频输出应该被静音。                               |
    | [poster](https://www.w3school.com.cn/tags/att_video_poster.asp) | *URL*    | 规定视频下载时显示的图像，或者在用户点击播放按钮前显示的图像。 |
    |                                                              |          |                                                              |
    | [src](https://www.w3school.com.cn/tags/att_video_src.asp)    | *url*    | 要播放的视频的 URL。                                         |
    | [width](https://www.w3school.com.cn/tags/att_video_width.asp) | *pixels* | 设置视频播放器的宽度。                                       |

    ==注意谷歌浏览器需要加上muted="muted"才能自动播放==

- input

  - autoplay  ,controls,src , loop常见属性

- form表单属性

  - #### 新的 input 属性：

    - ==required==  :输入框中的小字

- css新增的属性选择器

  - ```css
    [attribute=value]
    
    input[type="button"]
    {
      width:120px;
      margin-left:35px;
      display:block;
      font-family: Verdana, Arial;
    }
    ```

  - 

- css结构伪类选择器

- 伪元素选择器

  - ::before
  - ::after
  - 伪元素选择器比在html中加一个无用的div代码更容易让人理解。



伪元素清除浮动

```css
<style>
div {
  border: 1px solid #ccc;
}

.y-clearfix:before, .y-clearfix:after {
  content: "";
  display: block;
  height: 0;
  overflow: hidden;
  clear: both;
}
</style>
<div class="y-clearfix">A
  <div style="float: left;height: 50px;">B</div>
</div>
```

//div里面的浮动元素始终在父盒子里面，尽管父盒子没有高度

box-size: border-box



==css过渡==(重点)

  transition: margin-left 2s;   

transition表示要绑定加的过渡效果的东西，谁需要就加谁

```css
 .red {
            width: 50px;
            height: 50px;
            background-color: red;
            /* visibility: hidden; */
            transition: margin-left 2s;//margin-left表示该属性要过渡效果
           float: left;
            margin-left: 0px
                /* transition: ; */
        }

 .box:hover .red{
            margin-left: 50px;   //过渡后的结果
        }
```



一般搭配hover 一起使用

## 2D转换

transform 

有如下方法

- transform: translateX(-50%);}==translate==  用于给绝对定位居中 下移自身的50%
- translateY();
- 旋转 transform: rotate(5deg);  表示旋转5度
- [translate()](https://li-xinyang.gitbooks.io/frontend-notebook/content/chapter1/04_07_transform.html#translate)表示平面移动，参数可以是(x,y )
- [scale()](https://li-xinyang.gitbooks.io/frontend-notebook/content/chapter1/04_07_transform.html#scale)  放大缩小参数(2,2)表示放大2倍
- [skew()](https://li-xinyang.gitbooks.io/frontend-notebook/content/chapter1/04_07_transform.html#skew)  其为倾斜的方法。第一值为 Y 轴往 X 方向倾斜（逆时针），第二值为 X 轴往 Y 方向倾斜（顺时针）。（倾斜值可为负值）
- 



transform-origin:20% 40%;   表示以什么为原点旋转，默认是50% 50% 中心点



## css动画

动画序列@ keyframs

```js
@keyframes myfirst{
    from{background: red;}
    to{background: yellow;}
}
```

from 可以用）0%代替  to 可以用100%代替

```css
     .bear{
            position: absolute;
            width: 200px;
            height: 100px;
            background-image: url(images/20200322220134546.png);
            animation: move 0.5s infinite steps(8),divmove 2s forwards;/*move是该动作的名称*/

        }
```

- infinite表示无限循环
- steps(8)表示分成八步，一步一步来 
- forwards 挺到当前状态



| 值                                                           | 描述                                     |
| ------------------------------------------------------------ | ---------------------------------------- |
| *[animation-name](https://www.w3cschool.cn/cssref/pr-animation-name.html)* | 规定需要绑定到选择器的 keyframe 名称。。 |
| *[animation-duration](https://www.w3cschool.cn/cssref/pr-animation-duration.html)* | 规定完成动画所花费的时间，以秒或毫秒计。 |
| *[animation-timing-function](https://www.w3cschool.cn/cssref/pr-animation-timing-function.html)* | 规定动画的速度曲线。                     |
| *[animation-delay](https://www.w3cschool.cn/cssref/pr-animation-delay.html)* | 规定在动画开始之前的延迟。               |
| *[animation-iteration-count](https://www.w3cschool.cn/cssref/pr-animation-iteration-count.html)* | 规定动画应该播放的次数。                 |
| *[animation-direction](https://www.w3cschool.cn/cssref/pr-animation-direction.html)* | 规定是否应该轮流反向播放动画。           |



*[animation-timing-function](https://www.w3cschool.cn/cssref/pr-animation-timing-function.html)*默认四ease ，先慢后块