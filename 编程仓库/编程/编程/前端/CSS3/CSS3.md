# CSS3 概述
CSS3 是 CSS2 的升级版本，它在 CSS2 的基础上，新增了很多强大的新功能，从而解决一些实际面临的问题。

CSS3 在未来会按照模块化的方式去发展： https://www.w3.org/Style/CSS/current-work.html

CSS3 的新特性如下：
	新增了更加实用的选择器，例如：动态伪类选择器、目标伪类选择器、伪元素选择器等等。
	新增了更好的视觉效果，例如：圆角、阴影、渐变等。
	新增了丰富的背景效果，例如：支持多个背景图片，同时新增了若干个背景相关的属性。
	新增了全新的布局方案 —— 弹性盒子
	新增了 Web 字体，可以显示用户电脑上没有安装的字体。
	增强了颜色，例如： HSL 、 HSLA 、 RGBA 几种新的颜色模式，新增 opacity 属性来控制透明度。
	增加了 2D 和 3D 变换，例如：旋转、扭曲、缩放、位移等。 
	增加动画与过渡效果，让效果的变换更具流线性、平滑性。
	.....

# CSS3私有前缀
## 什么是私有前缀
如下代码中的 -webkit- 就是私有前缀
```css
div {
width:400px;
height:400px;
-webkit-border-radius: 20px;
}

```

## 为什么要有私有前缀
W3C 标准所提出的某个 CSS 特性，在被浏览器正式支持之前，浏览器厂商会根据浏览器的内核，使用私有前缀来测试该 CSS 特性，在浏览器正式支持该 CSS 特性后，就不需要私有前缀了。
举个例子：
```css

-webkit-border-radius: 20px;
-moz-border-radius: 20px;
-ms-border-radius: 20px;
-o-border-radius: 20px;
border-radius: 20px;


```

## 常见浏览器私有前缀
Chrome 浏览器： -webkit
Safari 浏览器： -webkit
Firefox 浏览器： -moz
Edge 浏览器： -webkit
旧 Opera 浏览器： -o
旧 IE 浏览器： -ms
>我们在编码时，不用过于关注浏览器私有前缀，不用绞尽脑汁的去记忆，也不用每个都去查询，因为常用的 CSS3 新特性，主流浏览器都是支持的，即便是为了老浏览器而加前缀，我们也可以借助现代的构建工具，去帮我们添加私有前缀。


# CSS3 基本语法
## CSS3 新增长度单位
1. rem 根元素字体大小的倍数，只与根元素字体大小有关。
2. vw 视口宽度的百分之多少 10vw 就是视口宽度的 10% 。
3. vh 视口高度的百分之多少 10vh 就是视口高度的 10% 。 
4. vmax 视口宽高中大的那个的百分之多少。（了解即可） 5. vmin 视口宽高中小的那个的百分之多少。（了解即可）


## CSS3 新增颜色设置方式
CSS3 新增了三种颜色设置方式，分别是： rgba 、 hsl 、 hsla ，由于之前已经详细讲解，此处略过。

## CSS3 新增选择器
CSS3 新增的选择器有：动态伪类、目标伪类、语言伪类、 UI 伪类、结构伪类、否定伪类、伪元素；这些在 CSS2 中已经详细讲解，此处略过。

## CSS3 新增盒模型相关属性
### box-sizing 怪异盒模型
使用` box-sizing` 属性可以设置盒模型的两种类型
怪异盒模型设置的长度和高度是总高度，当设置边框机会压缩内容区。
![](imgs/Pasted%20image%2020230617202851.png)

###  resize 调整盒子大小
使用 resize 属性可以控制是否允许用户调节元素尺寸。
![](imgs/Pasted%20image%2020230617203645.png)

### box-shadow 盒子阴影
使用 box-shadow 属性为盒子添加阴影。
```css
box-shadow: h-shadow v-shadow blur spread color inset;
```
各个值的含义：
![](imgs/Pasted%20image%2020230617210830.png)

默认值： box-shadow: none 表示没有阴影

```css
/* 写两个值，含义：水平位置、垂直位置 */
box-shadow: 10px 10px;
/* 写三个值，含义：水平位置、垂直位置、颜色 */
box-shadow: 10px 10px red
/* 写三个值，含义：水平位置、垂直位置、模糊值 */
box-shadow: 10px 10px 10px;
/* 写四个值，含义：水平位置、垂直位置、模糊值、颜色 */
box-shadow: 10px 10px 10px red;
/* 写五个值，含义：水平位置、垂直位置、模糊值、外延值、颜色 */
box-shadow: 10px 10px 10px 10px blue;
/* 写六个值，含义：水平位置、垂直位置、模糊值、外延值、颜色、内阴影 */
box-shadow: 10px 10px 20px 3px blue inset
```

### opacity 不透明度
**opacity** 属性能为整个元素添加透明效果，值是 0 到 1 之间的小数， 0 是完全透明， 1 表示完全不透明。

opacity 与 rgba 的区别？
opacity 是一个属性，设置的是整个元素（包括元素里的内容）的不透明度。 
rgba 是颜色的设置方式，用于设置颜色，它的透明度，仅仅是调整颜色的透明度。

# CSS3 新增背景属性
## background-origin
作用：设置背景图的原点。
1. padding-box ：从 padding 区域开始显示背景图像。—— 默认值 
2. border-box ： 从 border 区域开始显示背景图像。 
3. content-box ： 从 content 区域开始显示背景图像。


## background -clip
作用：设置背景图的向外裁剪的区域。
1. border-box ： 从 border 区域开始向外裁剪背景。 —— 默认值 
2. padding-box ： 从 padding 区域开始向外裁剪背景。 
3. content-box ： 从 content 区域开始向外裁剪背景。 
4. text ：背景图只呈现在文字上。

>注意：若值为 text ，那么 backgroun-clip 要加上 -webkit- 前缀。


## background-size
作用：设置背景图的尺寸。
用长度值指定背景图片大小，不允许负值。
```css
background-size: 300px 200px;
```

用百分比指定背景图片大小，不允许负值。
```css
background-size: 100% 100%;
```

auto ： 背景图片的真实大小。 —— 默认值

contain ： 将背景图片等比缩放，使背景图片的宽或高，与容器的宽或高相等，再将完整背景图片包含在容器内，但要注意：可能会造成容器里部分区域没有背景图片。
```css
background-size: contain;
```

cover ：将背景图片等比缩放，直到完全覆盖容器，图片会尽可能全的显示在元素上，但要注意：背景图片有可能显示不完整。—— 相对比较好的选择
```css
background-size: cover;
```

## backgorund 复合属性



