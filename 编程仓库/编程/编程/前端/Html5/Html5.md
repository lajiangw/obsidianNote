# HTML5 优势
1.  针对 JavaScript ，新增了很多可操作的接口。
2. 新增了一些语义化标签、全局属性。
3. 新增了多媒体标签，可以很好的替代 flash 。
4. 更加侧重语义化，对于 SEO 更友好。
5. 可移植性好，可以大量应用在移动设备上。


# HTML5兼容性
支持： Chrome 、 Safari 、 Opera 、 Firefox 等主流浏览器。

# 新增语义化标签
## 1. 新增布局标签
![](imgs/Pasted%20image%2020230617145553.png)

关于 **article** 和 **section** ：
>1. artical 里面可以有多个 section 。 
>2. section 强调的是分段或分块，如果你想将一块内容分成几段的时候，可使用 section 元素。 
>3. article 比 section 更强调独立性，一块内容如果比较独立、比较完整，应该使用 article 元素。


# 新增状态标签
### meter 标签
语义：定义已知范围内的标量测量。也被称为 gauge （尺度），双标签，例如：电量、磁盘用量等。

常用属性如下:
![](imgs/Pasted%20image%2020230617145817.png)

### progress 标签
语义：显示某个任务完成的进度的指示器，一般用于表示进度条，双标签，例如：工作完成进度等。
![](imgs/Pasted%20image%2020230617151517.png)

### 新增列表标签
![](imgs/Pasted%20image%2020230617151538.png)

演示：
**Datalist**
```html
<form action="#">
            <input type="text" list="list">
            <button type="submit">搜索</button>
        </form>
        <datalist id="list">
            <option value="周冬雨">周冬雨</option>
            <option value="周杰伦">周杰伦</option>
            <option value="马冬梅">马冬梅</option>
        </datalist>
```

**details**
```html
  <details>
            <summary>你是谁？</summary>
            <p>我是好人</p>
        </details>
```

### 新增文本标签
#### 文本注音
![](imgs/Pasted%20image%2020230617183456.png)

```html
<ruby>
<span>魑魅魍魉</span>
<rt>chī mèi wǎng liǎng </rt>
</ruby>

```

# 新增表单功能
## 表单控件新增属性
![](imgs/Pasted%20image%2020230617183715.png)

##  input 新增属性值
![](imgs/Pasted%20image%2020230617184422.png)

## form 标签新增属性
![](imgs/Pasted%20image%2020230617184436.png)

# 新增多媒体标签
## 视频标签
**< video >** 标签用来定义视频，它是双标签

![](imgs/Pasted%20image%2020230617184915.png)

## 音频标签
**< audio >** 标签用来定义音频，它是双标签。
自动播放只有静音才可以自动播放。
如果没有静音就播放了说明**媒体参与度比较高**，也是是你打开这个网址经常直接点击播放。浏览器就会允许不静音自动播放。
可以在**chrome://media-engagement** 内部页面上找到用户的MEI
![](imgs/Pasted%20image%2020230617185426.png)

# 新增全局属性（了解）
![](imgs/Pasted%20image%2020230617185439.png)

# HTML5兼容性处理
添加元信息，让浏览器处于最优渲染模式
```html
<!--设置IE总是使用最新的文档模式进行渲染-->
<meta http-equiv="X-UA-Compatible" content="IE=Edge">

<!--优先使用 webkit ( Chromium ) 内核进行渲染, 针对360等壳浏览器-->
<meta name="renderer" content="webkit">
```

使用 html5shiv 让低版本浏览器认识 H5 的语义化标签。
标准格式，不能改变。
```html
<!--[if lt ie 9]>
<script src="../sources/js/html5shiv.js"></script>
<![endif]-->
```

扩展
>lt 小于 lte 小于等于 gt 大于 gte 大于等于 ! 逻辑非


```html
<!--[if IE 8]>仅IE8可见<![endif]-->
<!--[if gt IE 8]>仅IE8以上可见<![endif]—>
<!--[if lt IE 8]>仅IE8以下可见<![endif]—>
<!--[if gte IE 8]>IE8及以上可见<![endif]—>
<!--[if lte IE 8]>IE8及以下可见<![endif]—>
<!--[if !IE 8]>非IE8的IE可见<![endif]-->
```






