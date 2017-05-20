# Vue

响应式的数据绑定。


### 数据驱动思想

1. 数据到试图的映射
1. 操作数据改变视图
1. 简化对DOM的操作

一旦修改了数据，立马更新试图

简洁的模板语法来声明式的将数据渲染进 DOM 系统

```
<div id="app">
    {{ message }}
</div>


var app = new Vue({  //创建Vue实例，传入一个选项对象。包含数据，挂载元素、方法等
    el: '#app',  //选择器
    data: {                      //指向Model
        message: 'Hello Vue!'
    }
})
```


### MVVM模式（Model-View-ViewModel）

当创建了ViewModel后，双向绑定是如何达成的呢？

首先，我们将上图中的DOM Listeners和Data Bindings看作两个工具，它们是实现双向绑定的关键。
从View侧看，ViewModel中的DOM Listeners工具会帮我们监测页面上DOM元素的变化，如果有变化，则更改Model中的数据；
从Model侧看，当我们更新Model中的数据时，Data Bindings工具会帮我们更新页面中的DOM元素。



## 双向绑定示例

MVVM模式本身是实现了双向绑定的，在Vue.js中可以使用`v-model`指令在表单元素上创建双向数据绑定。

### v-model 

```
<div id="app">
    <p>{{ message }}</p>
    <input type="text" v-model="message"/>
</div>
```
更改input框中的内容时，p标签里的内容会实时发生变化


### Vue中的循环和判断

通过 指令

 

```
graph LR
v-for指令-->循环
```

语法：
```
v-for='value,index in list'

```
list是一个数组

```
<div id='box'>
    <h2>{{title}}</h2>
    <div>{{content}}</div>
    <ul>
        <li v-for='value,index in list'>{{index}}-{{value}}</li>
    </ul>
</div>

<script src='js/vue.js'></script>
<script>
    //for 循环指令
    
    var o=new Vue({
    //绑定视图
        el:'#box',
        data:{
            title:'这是标题',
            content:'这是正文部分',
            list:['Jack','Bill','Tracy','Chris']
        }
    })		

</script>
```



### v-bind 行间绑定属性

可以简写成 `:`

绑定行间的src，css，style，href等
```
<img v-bind:src="value.imgSrc" />

可以简写成

<img :src="value.imgSrc" />
```

###  v-on 监听事件

行间的事件函数放到`methods`里


```
<!--click事件直接绑定一个方法-->
<button v-on:click="greet($event)">按钮</button>
```
给按钮绑定一个greet的方法,写到`method`里面，可以传一个默认的全局的事件对象 `$event`






```
var vm=new Vue({
    //绑定视图
    el:'#box',
    data:{
        title:'这是标题',
        content:'这是正文部分',
        list:['Jack','Bill','Tracy','Chris'],
        methods:{
             greet: function(event) {
            // 方法内 `this` 指向 vm 即Vue实例
                alert(this.message)
                
                alert(event.target)
            }
        }
    }
})
```
可以弹出事件的`event.target	`
	


## 事件修饰符

`methods `只有纯粹的数据逻辑，而不是去处理 `DOM `事件细节。为了解决这个问题， `Vue.js` 为 `v-on`
提供了 事件修饰符。通过由点(.)表示的指令后缀来调用修饰符。


```
<input v-on:keyup.13="submit">

<input v-on:keyup.enter="submit">  

<input @keyup.enter="submit">  

<input @click.stop="submit"> 取消冒泡  


<input @click.prevent="submit"> 阻止默认行为
```
只有在发生键盘事件并且按下enter键的时候，才触发submit方法

## v-show指令 显示，隐藏 
控制元素的`display css`属性


` v-show` 指令的取值为`true/false`，分别对应着显示/隐藏。
```
v-show='false' 元素会display:none;

v-show='true' 元素会display:block;
```


```
<div id='box'>
    <input type="checkbox" id="checkbox" v-model="checked">
    <label v-show='checked'>勾上就会显示</label>		
</div>
```


## v-if指令

v-if是条件渲染指令，它根据表达式的真假来删除和插入元素。



```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <div id="app">
            <h1>Hello, Vue.js!</h1>
            <h1 v-if="yes">Yes!</h1>
            <h1 v-if="no">No!</h1>
            <h1 v-if="age >= 25">Age: {{ age }}</h1>
            <h1 v-if="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
        </div>
    </body>
    <script src="js/vue.js"></script>
    <script>
        
        var vm = new Vue({
            el: '#app',
            data: {
                yes: true,
                no: false,
                age: 28,
                name: 'keepfool'
            }
        })
    </script>
</html>
```

这段代码使用了4个表达式：

1. 数据的yes属性为true，所以"Yes!"会被输出；
1. 数据的no属性为false，所以"No!"不会被输出；
1. 运算式age >= 25返回true，所以"Age: 28"会被输出；
1. 运算式name.indexOf('jack') >= 0返回false，所以"Name: keepfool"不会被输出。


## 添加class


```
<div v-bind:class="{ active: isActive }"></div>

面的语法表示 classactive 的更新将取决于数据属性 isActive 是否为真值 。
```


true的话才会添加上名为red的class属性

false的话不会添加上名为red的class属性


```
<p class="fontSize" :class="{red:!list.length,blue:list.length}">新闻列表</p>
```
list.length为真的话，p添加blue的class属性，反之，添加red的class属性


get和set
computed里面有个reverseMsg。

get的时候 reverseMsg 是不能修改的。因为默认的是getter