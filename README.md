# Responsive-Web-Design
响应式设计应该是老生常谈的话题了，对于它的解决方案，近年来层出不穷。作为现在才来研究的前端小白，的确有些惭愧了。
废话不多说，直入正题。<br/>
响应式设计我个人认为分为PC端自适应和移动端适配，两个方案的解决思路和呈现结果有所不同
### PC端自适应
PC端自适应布局参考的是bootstrap，利用的是媒体查询和百分比。
接下来介绍一个bootstrap的自适应远离
#### 媒体查询
媒体查询其实就是限定宽度（设备屏幕宽度或者浏览器宽度），在不同的宽度下，呈现不同的属性值
比如：<br/>
在css文件中：
````
 /*屏幕宽度>=的时候,但由于后面那句，所以又<1266px*/
 @media (min-width: 768px){
            .container{
                width: 768px;
            }
        }
        /*屏幕宽度>=1266的时候*/
        @media  (min-width: 1200px){
            .container{
                width: 1200px;
            }
        }
````
第一句的意思是：在浏览器宽度最小是768px的时候，就是浏览器宽度在\[768px,无穷大]的情况，container这个类的width=768px;
同理，第二句话的意思是：在浏览器宽度\[1200px , 无穷大]的情况下，类container的width=1200px。综合两种情况，就是

@media后面跟的属性值有挺多，属性之间用`and`隔开表示`&&`运算，用逗号`,`隔开表示`||`预算，常见的media的属性值如下：
````
width:浏览器可视宽度。
height:浏览器可视高度。
device-width:设备屏幕的宽度。
device-height:设备屏幕的高度。
orientation:检测设备目前处于横向还是纵向状态。
aspect-ratio:检测浏览器可视宽度和高度的比例。(例如：aspect-ratio:16/9)
device-aspect-ratio:检测设备的宽度和高度的比例。

color:检测颜色的位数。（例如：min-color:32就会检测设备是否拥有32位颜色）
color-index:检查设备颜色索引表中的颜色，他的值不能是负数。
monochrome:检测单色楨缓冲区域中的每个像素的位数。（这个太高级，估计咱很少会用的到）
resolution:检测屏幕或打印机的分辨率。(例如：min-resolution:300dpi或min-resolution:118dpcm)。
grid：检测输出的设备是网格的还是位图设备。
-webkit-min-device-pixel-ratio:是设备上物理像素和设备独立像素(device-independent pixels (dips))的比例
(如iphone5设备像素比为：320X568,device-pixel-ratio的值为2，所以截图放到电脑上看，图片尺寸为640X1136，这个是在移动端使用，但现在有别的方案[代替](http://www.zhangxinxu.com/wordpress/2012/08/window-devicepixelratio/))。
````
比较常用的几个属性
````

@media screen and (min-width:600px) and (max-width:700px){...}--宽度在600px和700px之间
@media screen and (min-device-width:600px) and (max-device-width:700px)--屏幕设备宽度在600~700px之间

````

比较常用的就是以上几个，更多media的属性可以看这篇[文章](http://www.tuicool.com/articles/vQzei2i)

而在bootstrap的栅格系统中，可以看到
有很多这样的写法
````
<div class="col-sm-12,col-md-6,col-lg-3">container</div>
````
但是我就在琢磨，它是怎么让不同屏幕宽度下显示不同的三个类的呢
于是，我看了下源码Sass的分析[文章](https://segmentfault.com/a/1190000002709850)（有兴趣的自己研究一下栅格系统，挺有意思的），
总结了写法，自己设置了三个类，分别控制背景颜色为red、green、blue；
同时添加在同一个div上，查看在不同的宽度下显示
````
html:
<div class="bg-red bg-green bg-blue"></div>
style.css:
        div{
            width:100px;
            height:100px;
        }
        .bg-red{
            background-color: red;
        }
        @media (min-width: 768px){
            .bg-blue{
                background-color: blue;
            }
        }
        @media  (min-width: 1200px){
            .bg-green{
                background-color: green;
            }
        }

````
PS：一般都会设置一个默认的属性（bg-red）,然后再设置不同宽度下的属性（bg-green/blue）

在chrome下查看，发现改变浏览器宽度,div显示了不同的背景色，也就实现了多个类在不同宽度下显示唯一显示。

基于以上原理：
我们可以用媒体查询+百分比来实现栅格系统。参考sass（https://segmentfault.com/a/1190000002709850）的写法。

* bootstrap中设置的屏幕宽度为：
````
$screen-xs: 480px !default; // 手机
  $screen-sm: 768px !default; // 平板
  $screen-md: 992 !default;  // 桌面计算机
  $screen-lg: 1200 !default;
 ````
* 默认栅格系统分为12格，每格占百分比为100/12=8.33333

````
//手机
col-xs-1{
    width:8.33333%
}
col-xs-2{
     width:16.66667%
}
...
col-xs-12{
    width:100%;
}
//平板
@media (mix-width:768px){
    col-sm-1{
        width:8.33333%
    }
    col-sm-2{
         width:16.66667%
    }
    ...
    col-sm-12{
        width:100%;
    }
}
//pc
@media (mix-width:992px){
    col-md-1{
        width:8.33333%
    }
    col-md-2{
         width:16.66667%
    }
    ...
    col-md-12{
        width:100%;
    }
}

//pc
@media (mix-width:992px){
    col-lg-1{
        width:8.33333%
    }
    col-lg-2{
         width:16.66667%
    }
    ...
    col-lg-12{
        width:100%;
    }
}
````
使用方式如：
````
<div class="col-sm-12 col-md-6 col-lg-3"></div>
````
就会根据不同的浏览器宽度，div呈现不同的宽度，做到自适应。
像宽度高度这些可以用百分比来定，但如果文字大小、行高等，也用百分比来定，那结果可能会很惨淡
所以，就引出了REM的方案
````
//先来个demo
//html
<div class="container">
    这是容器
    <div class="title">这是标题</div>
</div>
//css
.container{
    font-size:100px;//父元素必须为固定大小单位，常用px
}
.title{
    font-size:.12em
    width:1em;
    height:.5em;
}
````
rem是css3出的新单位，是为了代替em出现，em是根据相对父元素的文字大小来定，如
以上代码，`.title`的字体大小取决于`.container`的字体大小，
其具体计算公式为：`1em=100px(100px为父元素的字体大小)`
咋一看这样貌似没什么问题，但这样做，在父元素嵌套很多层的情况下，就变得尤为复杂，不利于维护
所以rem就诞生了！<br/>
rem全称是root-em，即以页面根元素为参考的em写法，而root一般代指`<html>`标签，也就是说：当我们设置好了`html`的字体大小之后，然后再根据媒体查询（或者js，下面会讲），去动态的改变
`<html>`的字体大小（必须为固定大小单位，如:px），
````
html{
    font-size:100px;
}
.container{
    font-size:.20rem;
    width:1rem;
    height:.5rem
    background-color:blue;
}
.title{
    font-size:.12rem;
    width:.5rem;
    height:.25rem;
    background-color:red;
}
````
用媒体查询来控制根元素的宽度
````
@media (min-width:420px){
    html{
        font-size:100px;
    }
}
@media (min-width:768px){
    html{
        font-size:130px;
    }
}
@media (min-width:992px){
    html{
        font-size:150px;
    }
}

````
媒体查询大致就介绍到这里吧，具体就靠大家自己去应用啦
### 移动端适配
* 方案一：flex布局+REM+JS
* 方案二：淘宝Flexible方案

#### flex布局+REM+JS
先贴出两篇大神的文章
* [手机端页面自适应解决方案—rem布局](http://www.jianshu.com/p/b00cd3506782)
* [Flex布局教程-阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

相信看过前面两篇教程，对这个方案有了大致的理解。时间的关系我就不一一解释了
对于flex的兼容性问题，可以看看这篇[文章](https://segmentfault.com/a/1190000003978624)

#### 淘宝Flexible方案
同样贴出两篇大神文章：
* [手机端页面自适应解决方案—rem布局进阶版（附源码示例）](http://www.jianshu.com/p/985d26b40199)
* [使用Flexible实现手淘H5页面的终端适配](https://github.com/amfe/article/issues/17)

这个方案与方案一的优势就在于：

* 引用简单，布局简便
* 根据设备屏幕的DPR,自动设置最合适的高清缩放。
* 保证了不同设备下视觉体验的一致性。（老方案是，屏幕越大元素越大；此方案是，屏幕越大，看的越多）
* 有效解决移动端真实1px问题（这里的1px 是设备屏幕上的物理像素）


具体文章也有demo，大家多去发现，多去踩坑，总会发现新大陆的

收工走人！溜了溜了~


