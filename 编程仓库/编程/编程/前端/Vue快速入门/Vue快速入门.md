# Vue 中文文档、
[简介 | Vue.js (vuejs.org)](https://cn.vuejs.org/guide/introduction.html#api-styles)

# 快速入门
第一个 Vue
```html
<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>titile</title>

</head>

<body>

    <div id="app">

        {{message}}

    </div>

  

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <script>

        var app = new Vue({

            el: "#app",

            data: {

                message: "你好,vue!"

            }

        })

    </script>

</body>

</html>
```

# El 挂载点
EL 挂载的范围为：只要在 EL 挂载点的 dom 元素内部，都是生效的。但是不可以挂载到 Body 和 HTML 元素上。
EL 只支持双标签，单标签的内部是无效的。

# Data 数据对象
Vue 使用的数据，我们都会放到 data 中。
Vue 可以写复杂的类型语法
Vue 中获取数据使用. 语法。

# Vue 一些常见指令
## V-text
设置标签的文本值（textContext）
我们直接使用 v-text 会导致所有的 text 属性都被替换，可以使用差值表达方法。
V-text 属性会把属性的文本信息全部替换，后续在标签添加会失效。这时我们可以切换 {{}} 形式
```html
  <div id="app">

        <h2 123{{message}}></h2>
			差值表达方式
			 <h2 v-text="message"></h2>
    </div>



    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <script>

        var app = new Vue({

            el: "#app",

            data: {

                message: "张三"

            }

        })

    </script>
```

## V-html
设置标签的 innerHTML
InnerHtml 可以设置一些其他属性的值，而 text 属性只代表文字。
只是文本推荐 v-text
如果有其他 html 元素使用 v-html
```html
<div id="app">

        <p v-html="content"></p>

        <p v-text="content"></p>

    </div>

  
  

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <script>

        var app = new Vue({

            el: "#app",

            data: {

                content: "<a href='https://baidu.com'>百度<a/>"

            }

        })

    </script>
```
可以通过预览图看出差距。
![[imgs/Pasted image 20230322233744.png]]

## V-on
为元素绑定事件
V-on 有两种使用方式，一种是@，一种是 v-on
当我们想在方法区中拿到 data 中的值，需要使用 `this` 关键字
```html
 <div id="app">

        <input type="button" value="v-on点击事件" v-on:click="doIt">

        <input type="button" value="简写" @click="doIt">

        <input type="button" value="双击" @dblclick="doIt">

        <h2 @click="eatfood">{{food}}</h2>

    </div>

  

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <script>

        var app = new Vue({

            el: "#app",

  

            data: {

                food: "西红柿炒蛋"

            },

            methods: {

                doIt: function () {

                    alert("做it")

                },

                eatfood: function () {

                    // console.log(this.food)

                    this.food += "好好吃"

                }

            },

        })

    </script>
```