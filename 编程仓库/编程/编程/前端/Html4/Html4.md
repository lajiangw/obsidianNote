## 文档声明
![](imgs/Pasted%20image%2020230414163229.png)

红色框代表着这个代表使用 Html 5 编写，浏览器会使用 H5 的格式去渲染。
**文档声明必须写在第一行。**

## 字符编码
计算机存储信息包括编码和解码，当我们使用不合理的编码和存码时会导致乱码。
所以我们需要选择相同的编码和解码。
浏览器默认使用UTF-8编码。

![](imgs/Pasted%20image%2020230414164424.png)
红色框代表解码方式。

## 设置网页语言
![](imgs/Pasted%20image%2020230414164922.png)

设置网页的语言，一般了解即可。

## HTML开发文档
[MDN Web Docs](https://developer.mozilla.org/zh-CN/)
[w3school 在线教程](https://www.w3school.com.cn/)
推荐第一个。用的比较多。

## 排版标签
排版标签有 H 标签，一般网页中，H1 最后只有一个。
H1 是最重要的。
H标题标签不可以嵌套。
P标签也不可以嵌套。
![](imgs/Pasted%20image%2020230414171230.png)

## 语义化标签
语义化：标签默认的效果不重要，语义最重要。
**标签的默认效果可以通过css修改。**
语义化优点：
1. 代码可读性强。
2. 有利于SEO优化，（搜索引擎优化）。
3. 方便设备解析，（屏幕阅读器，盲人阅读器）


## 块级元素与行内元素
块级元素：独占一行，例如 H1标签等。
行内元素：不独占一行，例如 input标签。

![](imgs/Pasted%20image%2020230414172913.png)

块级元素规则：块级元素中几乎什么元素都能写。

行内元素规则：行内元素可以写行内元素，但不能写块级元素。

## 文本标签_常用的
用于包裹：词汇，短语等。
通常写在排版标签里面。
排版标签更宏伟，*文本标签更微观。*
文本标签通常都是行内元素。

![](imgs/Pasted%20image%2020230414211206.png)

>生活中的例子： div 是大包装袋， span 是小包装袋。


不常用的文本标签：
![](imgs/Pasted%20image%2020230414211301.png)

>	 这些不常用的文本标签，编码时不用过于纠结（酌情而定，不用也没毛病）。
>	 blockquote 与 address 是块级元素，其他的文本标签，都是行内元素。
>	 有些语义感不强的标签，我们很少使用，例如： small 、 b 、 u 、 q 、 blockquote
>	 HTML标签太多了！记住那些：重要的、语义感强的标签即可；截止目前，有这些： `h1~h6` 、` p `、 `div` 、 `em` 、 `strong` 、 `span`。


## 图片标签
### 基本使用：
![](imgs/Pasted%20image%2020230414212149.png)

>像素（ px ）是一种单位，
>尽量不同时修改图片的宽和高，可能会造成比例失调。
>**修改图片的宽和高时，只调整一个属性就会等比例方法缩小**
>暂且认为 img 是行内元素（学到 CSS 时，会认识一个新的元素分类，目前咱们只知道： 块、行内）。
>alt 属性的作用：
>	搜索引擎通过 alt 属性，得知图片的内容。—— 最主要的作用。
>	当图片无法展示时候，有些浏览器会呈现 alt 属性的值。
>	盲人阅读器会朗读 alt 属性的值。


### 路径的分类
### 相对路径
：是以**当前位置**作为参考点，去建立路径。
![](imgs/Pasted%20image%2020230414212258.png)

`./` 代表的是当前路径。
`../` 代表的是当前路径的上一级路径。
>细节问题：
	相对路径中的 ./ 可以省略不写。
	相对路径依赖的是当前位置，后期若调整了文件位置，那么文件中的路径也要修改。


### 绝对路径
：以**根位置**作为参考点，去建立路径。
本地绝对路径： E:/a/b/c/奥特曼.jpg 。（很少使用）
网络绝对路径： http://www.atguigu.com/images/index_new/logo.png 。

>	使用本地绝对路径，一旦更换设备，路径处理起来比较麻烦，所以很少使用。
>	使用网络绝对路径，确实方便，但要注意：若服务器开启了防盗链，会造成图片引入 失败。


### 网络路径
将一张图片变成网络，输入网络链接就可以使用这张图片。
有的图片会使用防盗链。不允许自己的网站使用。

## 超链接
主要作用：从当前页面进行跳转。
可以实现：
①跳转到指定页面
②跳转到指定文件（也可触发下载）
③跳转到锚点位置
④唤起指定 应用。
![](imgs/Pasted%20image%2020230420191439.png)

```html
   <!-- 跳转链接 -->

    <a href="https://www.baidu.com/" target="_blank">百度 </a>

    <!-- 跳转本地图片 -->

    <a href="./图片.html">跳转本地图片</a>
```

>1. 代码中的多个空格、多个回车，都会被浏览器解析成一个空格！
>2. **虽然 a 是行内元素，但 a 元素可以包裹除它自身外的任何元素！**


### 超链接跳转文件
浏览器跳转到可以查看的内容时会自动查看。
当跳转到浏览器无法识别的文件，会触发浏览器的下载。
当然也支持强制下载，使用 `download` 属性。
```html
 <!-- 强制下载 -->
    <a href="./imgs/123.png" download=""></a>
```

## 设置锚点
什么是锚点？—— **网页中的一个标记点。**
具体使用方式：

```html
<a href="#Wind10">跳转百度超链接</a>

    <p>Wind10</p>
    <!-- 使用id设置锚点 常用-->
    <img id="Wind10" src="./imgs/123.png" alt="Wind10">
    
    <!-- 使用a标签作为锚点 -->
    <a name="baidu"></a>
    
    <p>百度</p>
    <a href="https://www.baidu.com/">baidu </a>
    <br>
    <!-- 回到顶部 -->
    <a href="#">回到顶部</a>

    <!-- 刷新网页 -->
    <a href="">刷新网页</a><a href="#Wind10">跳转百度超链接</a>

    <p>Wind10</p>

    <!-- 使用id设置锚点 常用-->

    <img id="Wind10" src="./imgs/123.png" alt="Wind10">

    <!-- 使用a标签作为锚点 -->

    <a name="baidu"></a>

    <p>百度</p>

    <a href="https://www.baidu.com/">baidu </a>

    <br>

    <!-- 回到顶部 -->

    <a href="#">回到顶部</a>

    <!-- 刷新网页 -->

    <a href="">刷新网页</a>
```

### 超链接唤起指定应用
通过 a 标签，可以唤起设备应用程序。
```html
<a href="tel:10010">打电话</a>
    <br>
    <a href="mailto:320961101@qq.com">邮箱联系</a>
    <br>
    <a href="sms:15532913206">短信联系</a>
```

## 列表
### 有序列表
概念：有顺序或侧重顺序的列表。
```html
<h2>把大象放进冰箱需要几步？</h2>
    <ol>
        <li>把冰箱门打开</li>
        <li>把大象放进去</li>
        <li>关注冰箱门</li>
    </ol>
```

### 无须列表
没有顺序的列表
```html
<h2>我喜欢的城市</h2>
    <ul>
        <li>成都</li>
        <li>北京</li>
        <li>长沙</li>
    </ul>
```

### 自定义列表
概念：所谓自定义列表，就是一个包含术语名称以及术语描述的列表。
一个 dl 就是一个自定义列表，一个 dt 就是一个术语名称，一个 dd 就是术语描述（可以有多 个）。
```html
<h2>城市的标志建筑</h2>
    <dl>
        <dt>河北</dt>
        <dd>狮子头</dd>
        <dt>北京</dt>
        <dd>天安门</dd>
    </dl>
```

### 列表的嵌套
列表中可以进行嵌套
```html
<h2>我喜欢的城市</h2>
    <ul>
        <li>成都</li>
        <li>
            <span>北京</span>
            <ul>
                <li>天安门</li>
                <li>故宫</li>
            </ul>
        </li>
        <li>长沙</li>
    </ul>
```

**li 标签属性最好不要单独使用**

## 表格
### 基本结构
一个完整的表格由：表格标题、表格头部、表格主体、表格脚注，四部分组成。
![](imgs/Pasted%20image%2020230420201031.png)

表格涉及到的标签：
`table` 表格
`caption` 表格标题
`thead` 表格头部
`tbody` 表格身体
`tfood` 表格注脚
`tr` 每一行
`th` 、`td` ：每一个单元格（备注：表格头部中用 `th` ，表格主体、表格脚注中用： `td` ）
```html
 <table border="1" cellspacing="0">

        <caption>学生信息</caption>

        <thead >

            <th>姓名</th>

            <th>性别</th>

            <th>年龄</th>

        </thead>

        <tbody>

            <tr>

                <td>张三</td>

                <td>男</td>

                <td>15</td>

            </tr>

  

            <tr>

                <td>李四</td>

                <td>男</td>

                <td>18</td>

            </tr>

  

            <tr>

                <td>王五</td>

                <td>男</td>

                <td>22</td>

            </tr>

  

        <tfoot>

            <tr>

                <td></td>

                <td></td>

  

                <td>共计3人</td>

            </tr>

        </tfoot>

        </tbody>

  

    </table>
```

![](imgs/Pasted%20image%2020230420202206.png)
### 常用属性
![](imgs/Pasted%20image%2020230420204242.png)

细节问题：
>2. 默认情况下，每列的宽度，得看这一列单元格最长的那个文字。 
>3. 给某个 th 或 td 设置了宽度之后，他们所在的那一列的宽度就确定了。
>4. 给某个 th 或 td 设置了高度之后，他们所在的那一行的高度就确定了。


### 跨行和垮列
1. `rowspan` ：指定要跨的行数。
2. `colspan `：指定要跨的列数。


思考跨行时，先够所有的列数和行数，不要着急跨行跨列。

```html
<table border="1">
        <caption>课程表</caption>
        <thead>
            <tr>
                <th colspan="5">周1</th>

                <th colspan="2">周6</th>
                <th>周7</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td rowspan="3">1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
            </tr>

            <tr>
               
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
            </tr>

            <tr>
            
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
            </tr>

            <tr>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
                <td>1</td>
            </tr>
        </tbody>
        </thead>
    </table>
```

## 常用标签补充
![](imgs/Pasted%20image%2020230420210232.png)

## 表单
概念：一个包含交互的区域，用于收集用户提供的数据。
![](imgs/Pasted%20image%2020230421175505.png)

### 常用表单控件
#### 文本输入框
```html
<input type="text">
```

常用属性如下： name 属性：数据的名称。
value 属性：输入框的默认输入值。
maxlength 属性：输入框最大可输入长度。

#### 密码输入框
```html
<input type="password">

```

常用属性如下： name 属性：数据的名称。
Value 属性：输入框的默认输入值。
Maxlength 属性：输入框最大可输入长度。

#### 单选框
```html
<input type="radio" name="sex" value="male">男
<input type="radio" name="sex" value="female">女

```

常用属性如下： name 属性：数据的名称，注意：想要单选效果，多个 radio 的 name 属性值要保持一致。 
value 属性：提交的数据值。 
checked 属性：让该单选按钮默认选中。

#### 复选框
```html
<input type="checkbox" name="hobby" value="smoke">抽烟
<input type="checkbox" name="hobby" value="drink">喝酒
<input type="checkbox" name="hobby" value="perm">烫头

```

常用属性如下：： name 属性：数据的名称。 
value 属性：提交的数据值。
checked 属性：让该复选框默认选中
#### 隐藏域
```html
<input type="hidden" name="tag" value="100">
```

用户不可见的一个输入区域，作用是： 提交表单的时候，携带一些固定的数据。 
name 属性：指定数据的名称。 
value 属性：指定的是真正提交的数据。

#### 提交按钮
```html
<input type="submit" value="点我提交表单">
<button>点我提交表单</button>

```

注意： 1. button 标签 type 属性的默认值是 submit 。
2. button 不要指定 name 属性 
3. input 标签编写的按钮，使用 value 属性指定按钮文字。

#### 重置按钮等控件
![](imgs/Pasted%20image%2020230421182251.png)

### Label 标签
`label ` 标签可与表单控件相关联，关联之后点击文字，与之对应的表单控件就会获取焦点。
两种与 `label ` 关联方式如下：
让 ` label` 标签的 ` for ` 属性的值等于表单控件的 id 。
把表单控件套在 ` label ` 标签的里面。
#### fieldset 与 legend 的使用
`fieldset` 可以为表单控件分组、 `legend` 标签是分组的标题。

```html
 <label for="username">账号:</label>
        <input type="text" maxlength="10" name="username" id="username"><br>
        <label>密码：
            <input type="password" maxlength="10" name="pwd" id="pwd">
        </label>
```

####  表单总结
![](imgs/Pasted%20image%2020230421190542.png)

## 框架标签
![](imgs/Pasted%20image%2020230421193116.png)

iframe 标签的实际应用：
1. 在网页中嵌入广告。
2. 与超链接或表单的 target 配合，展示不同的内容。

## 字符实体
在 HTML 中我们可以用一种 `特殊的形式` 的内容，来表示某个符号，这种特殊形式的内容，就是 HTML 实体。比如小于号 < 用于定义 HTML 标签的开始。如果我们希望浏览器正确地显示这些字符，我们必须在 HTML 源码中插入字符实体。
总结概括来说就是来显示关键字的转义字符。

>字符实体由三部分组成：一个` &` 和一个`实体名称`（或者一个 # 和一个实体编号），最后加上一个分号` ; `。

![](imgs/Pasted%20image%2020230421193806.png)

##   HTML 全局属性
概念：什么标签都可以使用的属性。
![](imgs/Pasted%20image%2020230421194019.png)
完整全局属性 [全局属性 - HTML（超文本标记语言） | MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes)

## Meta 元属性
```html
1. 置字符编码
<meta charset="utf-8">
2. 针对 IE 浏览器的兼容性配置。
<meta http-equiv="X-UA-Compatible" content="IE=edge">
3. 针对移动端的配置（移动端课程中会详细讲解）
<meta name="viewport" content="width=device-width, initial-scale=1.0">
4. 配置网页关键字
<meta name="keywords" content="8-12个以英文逗号隔开的单词/词语">
5. 配置网页描述信息
<meta name="description" content="80字以内的一段话，与网站内容相关">
6. 针对搜索引擎爬虫配置：
<meta name="robots" content="此处可选值见下表">

```
![](imgs/Pasted%20image%2020230421195211.png)

```html
7. 配置网页作者：
<meta name="author" content="tony">
8. 配置网页生成工具
<meta name="generator" content="Visual Studio Code">
9. 配置定义网页版权信息：
<meta name="copyright" content="2023-2027©版权所有">
10. 配置网页自动刷新
<meta http-equiv="refresh" content="10;url=http://www.baidu.com">
```
