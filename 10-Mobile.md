# 移动端

## 适配

### 问题

H5页面要适配的终端设备数据
![image.png](https://upload-images.jianshu.io/upload_images/6784887-f6d7fce5a3046132.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这么多的设备屏幕有大有小，能承载的内容不尽相同，于是开发者就想如何写一个页面能尽量适配更多的设备

乔布斯在 iPhone4 的发布会上首次提出了Retina Display(视网膜屏幕)的概念，在 iPhone4 使用的视网膜屏幕中，把 2x2 个像素当1个像素使用，这样让屏幕看起来更精致，但是元素的大小却不会改变。

用一张图来解释：

![image.png](https://upload-images.jianshu.io/upload_images/6784887-3ec0f6dcdc72f6c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

于是市面上不同的手机只要更改相应的设备像素比就可以让同一个页面显示类似的效果，但是这些设备像素虽然差别跨度不大，还是有差别，于是便诞生了不同的移动端适配方案



### 开发

手机端web的开发通常为以下两种：

1、 响应式 （根据客户端的类型 pc pad phone）网页自动做适配以适应客户端的浏览

2、 针对移动端专门实现相应的页面，比如   m.jd.com    m.taobao.com  m.toutiao.com

两者区别： 专门针对移动端开发的页面 UI 效果更好，自由度更高



## 视口 viewport
**1、 什么是 `Viewport`**

手机浏览器是把页面放在一个虚拟的“窗口”（`viewport`）中，通常这个虚拟的“窗口”比屏幕宽，这样就不用把每个网页挤到很小的窗口中（这样会破坏没有针对手机浏览器优化的网页的布局），用户可以通过平移和缩放来看网页的不同部分。移动版的 Safari 浏览器最先引进了 `viewport` 这个 `meta tag`，让网页开发者来控制 `viewport` 的大小和缩放，其他手机浏览器相继支持。

2、 一个常用的针对移动网页优化过的页面的 viewport meta 标签大致如下：

`<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1,user-scalable=no">`

```js
meta 是给浏览器识别的
width：控制 viewport 的大小，可以指定的一个值，如600，或者其它特殊的值，如 device-width 为设备的宽度（单位为缩放为 100% 时的 CSS 的像素）。
height：和 width 相对应，指定高度。
initial-scale：初始缩放比例，也即是当页面第一次 load 的时候缩放比例。
maximum-scale：允许用户缩放到的最大比例。
minimum-scale：允许用户缩放到的最小比例。
user-scalable：用户是否可以手动缩放
```



## 布局单位
rem 是基于html节点的 `font-size` 大小的比例

em 基于当前元素字体大小的比例



## 适配原理
通常情况下，UI设计师在设计移动端的时候 一般会按照375px、750px的宽度作图，在实现的时候，我们设置 页面根元素的 font-size 为设计稿的1/10宽度，在不同的设备下，再通过 JS 动态的修改页面根元素的 font-size 即可实现。
		
如果按照设计稿100%呈现，而且代码中使用rem处理，那么适配的问题的关键即：需要动态根据移动端手机的屏幕宽度来修改 html 的 font-size 大小

```js
//当窗口大小重新发生变化的时候触发
window.onresize = function(){
	resizeFont();
}
resizeFont();
//重置当前rem的基本参照字体大小
function resizeFont(){
	//当前设备宽度
	var _w = window.screen.width;
	document.documentElement.style.fontSize = _w/10 + 'px';
}
```



vscode 编辑器可以添加 cssrem 插件，然后在设置中搜索 cssrem.rootFontSize，更改基础字体大小，即可在页面使用，如果不能使用，重启编辑器即可

sublime 编辑器需要 lib 文件夹下的 cssrem-master 压缩包解压，或者从[官网](https://github.com/flashlizi/cssrem)下载，然后执行：

- 进入packages目录：Sublime Text -> Preferences -> Browse Packages...
- 复制下载的 cssrem 目录到刚才的packges目录里
- 重启Sublime Text

+ 配置参数

  参数配置文件：Sublime Text -> Preferences -> Package Settings -> cssrem

  + `px_to_rem` - px转rem的单位比例，默认为40。
  + `max_rem_fraction_length` - px转rem的小数部分的最大长度。默认为6。
  + `available_file_types` - 启用此插件的文件类型。默认为：[".css", ".less", ".sass"]



## 移动端的精灵图

正常来讲设计稿按照750(2x图)设计的，px=>rem的基数设计也是设置为75的
但是在某些情况下,假如设计稿按照375(1x图)设计了，需要动态修改px=>rem的基数为37.5

设置图片宽度 因为里面的元素都是rem单位，是倍数单位，所以希望背景图片也是倍数单位
针对图片的处理：由于设计稿的缘故，所以需要具体查看切出来的图片的实际大小是2X还是1X

在高清显示屏中的位图被放大，图片会变得模糊，因此移动端的视觉稿通常会设计为传统PC的2倍

那么，前端的应对方案是：

设计稿切出来的图片长宽保证为偶数，并使用backgroud-size把图片缩小为原来的1/2

//例如图片宽高为：200px*200px，那么写法如下
`.css{width:100px;height:100px;background-size:100px 100px;}`





## 扩充知识：

**屏幕分辨率：**

屏幕分辨率是指纵横向上的像素点数，单位是px。屏幕分辨率确定计算机屏幕上显示多少信息的设置，以水平和垂直像素来衡量。就相同大小的屏幕而言，当屏幕分辨率低时（例如 640 x 480），在屏幕上显示的像素少，单个像素尺寸比较大。屏幕分辨率高时（例如 1600 x 1200），在屏幕上显示的像素多，单个像素尺寸比较小。



**Retina 显示屏：**

一种具备超高像素密度的液晶屏，同样大小的屏幕上显示的像素点由1个变为多个，如在同样宽度下的屏幕上，苹果设备的retina显示屏中，像素点1个变为4个。



**屏幕尺寸：**

屏幕尺寸是以屏幕对角线的长度来计量，计量单位为英寸。



**物理像素(physical pixel)：**
物理像素又被称为设备像素，他是显示设备中一个最微小的物理部件。每个像素可以根据操作系统设置自己的颜色和亮度。正是这些设备像素的微小距离欺骗了我们肉眼看到的图像效果。如下图早期的手机显示，颗粒感相当大。

![image-20200511132710966](C:\Users\moxua\AppData\Roaming\Typora\typora-user-images\image-20200511132710966.png)



**设备独立像素(density-independent pixel)：**
设备独立像素也称为密度无关像素，可以认为是计算机坐标系统中的一个点，这个点代表一个可以由程序使用的虚拟像素(比如说CSS像素)，然后由相关系统转换为物理像素。



**设备像素比(device pixel ratio)：**
设备像素比简称为dpr，其定义了物理像素和设备独立像素的对应关系。它的值可以按下面的公式计算得到：

```
设备像素比 ＝ 物理像素 / 设备独立像素
```

在JavaScript中，可以通过`window.devicePixelRatio`获取到当前设备的dpr。而在CSS中，可以通过`-webkit-device-pixel-ratio`，`-webkit-min-device-pixel-ratio`和 `-webkit-max-device-pixel-ratio`进行媒体查询，对不同dpr的设备，做一些样式适配。



**经典的1px边框问题：**

当我们css里写的1px的时候，由于它是逻辑像素，导致我们的逻辑像素根据这个设备像素比（dpr）去映射到设备上就为2px，或者3px，由于每个设备的屏幕尺寸不一样，就导致每个物理像素渲染出来的大小也不同（记得上面的知识点吗，设备的像素大小是不固定的），这样如果在尺寸比较大的设备上，1px渲染出来的样子相当的粗矿，这就是经典的一像素边框问题

最佳解决方法：

```css
div {
    height:1px;
    background:#000;
    -webkit-transform: scaleY(0.5);
    -webkit-transform-origin:0 0;
    overflow: hidden;
}

/* 2倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 2.0) {
    .border-bottom::after {
        -webkit-transform: scaleY(0.5);
        transform: scaleY(0.5);
    }
}

/* 3倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 3.0) {
    .border-bottom::after {
        -webkit-transform: scaleY(0.33);
        transform: scaleY(0.33);
    }
}
```







**苹果的适配图**
![image.png](https://upload-images.jianshu.io/upload_images/6784887-2287fb2d323e3b5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

众所周知，iPhone6的设备宽度和高度为375pt * 667pt,可以理解为设备的独立像素；而其dpr为2，根据上面公式，我们可以很轻松得知其物理像素为750pt * 1334pt。



