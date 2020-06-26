# 1   Vue

## 1.1  Vue的定义

l Vue.js------ 渐进式框架。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注视图层，并且非常容易学习，非常容易与其它库或已有项目整合(非常容易导入第三方轮子)。另一方面，Vue 完全有能力驱动采用单文件组件和Vue生态系统支持开发复杂单页应用。

l 渐进式:从核心到完备的全家桶(需要什么引用什么)

Vue本身语法内容并不多, 生态是很庞大的

l 增量:从少到多,从一页到多页,从简单到复杂



l  单文件组件: 一个文件描述一个组件

l  单页应用: 经过打包生成一个单页的html文件和一些js文件

为什么要讲vue? 项目前端都是Vue, 国内前端框架市场基本上70%以上都是使用vue这个框架

或者, 不是用框架写的前端 ( 旧的项目jsp 模板技术  )

框架:  工程化的角度

所谓框架, 就是在工程化实现一个东西的时候, 构建的大的轮廓, 和提供一些基础性的设施

后来者,或者使用者, 可以在这个框架的基础上, 迅速的把要实现的目标实体快速构建出来

 

l vue基本语法(简单)

l 简洁,轻量,快速,数据驱动,模块友好,组件化

 

现在的前端是组件化的天下

 

## 1.2  Vue课程

分为两部分

1, 先讲vue基本语法

2, 怎么创建项目--在vue中开发页面--打包项目--怎么部署

 

在html页面把vue基本语法讲清楚, 先借助html页面来实现vue

是有问题, 

 

.vue来实现

 

## 1.3  一个Vue页面用html

```html
 <!--一个div他的id是root -->
 <!--在vue中 {{}}:叫插值表达式，
\这个插值表达式之中是一个js式子,
\这个式子的对应参数要去对应关联的vue对象中去取-->
 <div  id="root">
   {{msg}}
   {{msg + aaa}}
 </div>
```

//这里new了一个vue对象

// 这个vue对象通过,对象的el属性,与el属性所指向的id所对应的html代码关联起来  

*//* 把对象和对应html域关联起来

// data标识vue对象的数据存储属性(除了vue对象固有的属性,别的属性一般存在data里)

```js
new Vue({
	el:"#root",
	data:{
		msg:123,
		aaa:"你好"
		}
	})
```



## 1.4  V指令

l V-bind

l V-model

l V-text

l V-html

l V-show

l V-if

l V-else

l V-else-if

l V-for

l V-on

l V-pre

l V-cloak

l v-once

## 1.5  V-bind: 单向绑定

 *<!--v-bind:* 单向绑定
     把vue对象中data里的值,绑定到html对应是属性上去,赋值给对应的属性
 *-->*

 

### 1.5.1 V-model: 双向绑定

l v-model:

l 实现双向数据绑定

l *<!--v-model:* 双向绑定, *改变或者绑定,是个双向的
 1,* 和v-bind作用相同,去对应的vue对象里的data取值,把取到的值赋值给所绑定的对象
 2, v-model绑定的参数,当这个参数**(**在html中)发生改变的时候, 他会同时去修改对应vue对象中data里面所对应的值

l 注意： 双向绑定v-model: *一般用于表单元素的value属性
     所以有时候可以省略的写* v-model="***"*
->

 

 

 

### 1.5.2 V-text v-html

类似于dom操作中的 innerText innerHTML

 

 

### 1.5.3 V-on:  事件监听

*<!--v-on:标识vue语法的事件监听
     把监听到的事件, 触发到* 对应vue对象的methods里面去
-->
 <!-- vue**中**没**有**onclick -->

 

*<!--注意1:* 在vue中, 如果要在方法或者别的钩子函数里, 要访问data里的值, 
	要通过this.参数名 来访问
     这个this就是指代这个vue对象
-->

 

 

### 1.5.4 V-show

隐藏和显示


 注意2:在v-show的判断条件里，可以写表达式

 

### 1.5.5 V-if

v-else-if  v-else

v-if和v-show的区别:  v-if逻辑中,如果不满足加载条件, 那么该元素不会挂载到dom树上去. 但是v-show 无论显示与否, 都会挂载到dom树上,是不过会显示和不显示

 

 

### 1.5.6 V-for

在vu中对应的html代码中创建一个循环结构

 

<!--注意1: 循环渲染的,到底是什么?	

循环渲染的,是v-for这个指令所在的html元素-->

 <!--注意2: v-for指令必须要和:key配套出现并且key值唯一,不可重复-->

```js
   "(aaa, index) in list":key(index)
   index代之循环遍历的下标
```
 <!--注意3：v-for指令可以通过 in/of关键字来进行遍历-->

 

 

### 1.5.7 V-pre

l v-pre:

l v-pre可以用来阻止预编译，有v-pre指令的标签内部的内容不会被编译，会原样输出。

### 1.5.8 v-cloak

l 延迟显示。

l 这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 [v-cloak] { display: none } 一起用时，这个指令可以隐藏未编译的 标签直到实例准备完毕

### 1.5.9 V-once

只加载一次(只编译一次)

 

 

### 1.5.10      回顾

v-bind: 单向绑定, 可以去vue对象的data里面取参数, 绑定到对应的属性上

v-model: 双向绑定 , 可以取值从Vue对象, 并且改变的话也会影响到vue对象中的值

 相互关联, 相互影响

v-text:  向标签内部插入一个字符串

v-html:  向标签内部插入一个可以是字符串,也可以是携带html标签的代码段

v-on:  事件监听, 可以缩写成 @ , 监听一个方法触发到vue对象的methods里面

v-show: 隐藏和显示, 一定会挂载到dom树上

v-if   也可以做隐藏和显示(分支结构)    不一定, 满足挂载, 不满足不挂载  

v-for: 循环渲染, 循环显示,  数组, for循环出来的是其所在标签一定要有个  :key=’唯一值’ In/of

v-pre: 阻止预编译

v-cloak: 延迟加载

v-once: 只加载一次

 

## 1.6  计算属性

一个属性是通过别的属性计算而来, 是个属性(虽然看起来, 看起来像个方法, 但是外在表现是个属性)

 

计算属性所计算的时机: 当它依赖的属性发生变化的时候, 那么它就会重新计算

 

不是存在于data里的一个参数, 存在computed

```vue
computed:{
sum:funcation(){
	return this.num1 + this.num2
	}
}
```

 

## 1.7  侦听器

监听器, 侦听一个vue对象中属性的变化, 然后触发一些操作

 

Watch: 是vue对象的一个固有属性, 里面写监听器

 注意: 我们如果想侦听那个参数发生改变,侦听器的方法名就是该参数

```html
watch: {
   msg: function () {
     this.changetimes ++
   }
 }
```

 

## 1.8  模板

Template: 模板是vue对象(此时此刻写法) 是一个以标签包裹的html代码块

模板会替换挂载的元素

```
new Vue({
	el:"#root"，
	template:"<div>我是一个div</div>"，
})
```

l 一个字符串模板作为 Vue 实例的标识使用。模板将会 替换 挂载的元素。挂载元素的内容都将被忽略(除非模板的内容有分发插槽 v-solt)。

 

 

## 1.9  组件

把一个整体的东西拆分成不同的东西, 

表现在我们前端里, 就是把一个页面拆分成不同的分块来实现

表现在我们vue里, 把一个页面, 拆分成多个vue对象来实现

在vue中更深层次的表现, 就是,把一个页面关联成一个vue对象(入口对象), 这个对象有一系列子对象(子对象通过某种途径和入口对象建立引用关系), 构建出一种类似于dom树结构, 组件树(对象树)

 

组件: 在vue中就是vue对象

 

 

 

 

 

## 1.10在html中有两种组件的写法

### 1.10.1      全局组件和局部组件

```vue
<div id = "root">
    <aaa></aaa>
    <b-bb></b-bb>
    <ccc></ccc>
</div>
<script>
    //全局组件：直接注册一个vue对象，在vue域中使用
    //通过Vue.component创建出一个vue对象，给这个vue对象起名字
    // Vue.component("aaa",{
    //     template:"<div>111</div>"
    // })
    // Vue.component("bbb",{
    //     template:"<div>222</div>"
    // })
    // Vue.component("ccc",{
    //     template:"<div>333</div>"
    // })
    //利用var 变量名 先创造对象，再在其父对象上面注册为vue对象
    var aaa = {
        template:"<div>123</div>"
    }
    var bbb = {
        template:"<div>456</div>"
    }
    var ccc = {
        template:"<div>789</div>"
    }
    new Vue({
        el:"#root",
        components:{
          aaa:aaa,
          bBb:bbb,
          ccc:ccc
        },
        data:{
        }
    })
</script>
```

 

### 1.10.2     前后端分离

前端和后端分离:  工程上的分离, (分为前端项目, 分为后端项目)

对于前后端不分离的来说, 数据和前端代码的结合----服务器

前后端不分离的项目(前端和后端属于一个项目)

前后端分离的写法:  数据结合-----浏览器

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image006.jpg)

 

### 1.10.3    组件间传值: 向下(父组件传给子组件)传值

Props

v-bind : 单向绑定

传值: 

 至少要分两步, 

第一步:递(父组件, 传递通过v-bind)  

 第二步: 接收(子组件接收, 通过 props)

```vue
<div id = "root">
    <first v-bind:aaa = "msg"></first>
</div>
<script>
    Vue.component("first",{
        props:["aaa"],
        template:"<div>{{aaa}}</div>"
    })
    new Vue({
        el:"#root",
        data:{
            msg:"123"
        }
    })
</script>
```

 

![img](file:///C:/Users/飒飒/AppData/Local/Temp/msohtmlclip1/01/clip_image010.jpg

 

### 1.10.4 组件间传值：向上(子组件传递给父组件)

也要有两个动作: 

1: 子组件通知父组件: emit

2: 父组件监听字组件: 自定义方法

```vue
<script>
    Vue.component("first",{
        props:["aaa"],
        template:"<div><div @click='click1'>{{aaa}}</div></div>",
        methods:{
            click1:function () {
                this.$emit("bbb")
            }
        }
    })
    new Vue({
        el:"#root",
        data:{
            msg:"123",
            list:["1","2","3","4"]
        },
        methods:{
            delete1:function () {
                alert(111)
            }
        }
    })
</script>
```

##  2.1	工程化的创建一个vue项目

### 2.1.1 第一步

安装node

检测是否安装成功

`node -v`

`npm -v`

###  2.1.2 第二步

安装cnpm   阿里
`npm install -g cnpm --registry=https://registry.npm.taobao.org`命令标识 
Install:  安装
-g:  全局安装(两个问题:  第一个,不要直接复制代码运行ppt  全局:  )
Cnpm:  要安装的包
--registry=https://registry.npm.taobao.org:  从那获得这个包

`Cnpm -v`

 ![image-20200617080213720](C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200617080213720.png)

​		

###  2.1.3 第三步 vue-cli

安装脚手架工具(vue-cli:  它可以帮助我们快速构建一个项目)

- `cnpm install -g @vue/cli `
- `cnpm install -g @vue/cli-init `
- `vue –V` (v必须大写)

###  2.1.4 第四步 webpack

模块打包

`cnpm install -g webpack`

### 2.1.5 第五步 创建项目

`Vue init webpack 项目名`

`cnpm install`

### 2.1.6 第六步 启动项目

`npm run dev`

### 2.1.7 分析目录结构



![image-20200617081314558](C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200617081314558.png)

 

![image-20200617081655867](C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200617081655867.png)

### 2.1.8 idea安装插件

![image-20200617081743725](C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200617081743725.png)

### 2.1.9 中央总线事务

Vuex: 主要用来存储数据

总线: 计算机硬件(cpu 内存, 外存)

先有接收方:  监听动作

```js
    import  Bus from '../bus'
    export default {
        name: "Left",
        methods:{
          click1:function (parm) {
              //给bus（中央总线事务）发出一个methods用来存储数据
             Bus.$emit("change",parm)
          }
        }
    }
```

再有触发方:  触发的动作

```js
   mounted() {
       	  //接收change并向其中放入传递的数据
          Bus.$on('change',parm => {
              //tag即为在data中的数据
              this.tag = parm
          })
      }
```



Vue对象，包， 插件 

分三步: 导入到项目

1, 下包(导包), 导入文件,

2, 在项目中配置,引入到项目 

3, 使用

### 2.1.10 Json

是一种数据格式，用来传输数据

Xml也用来传输数据

### 2.1.11 接口文档

提供一个文档：描述一个接口

url: 三部分: 第三部分: 给服务器看的,

### 2.1.12 Axios

装包: `cnpm install axios --save`

导入: 引入到项目中去

`import axios from 'axios'`
`Vue.prototype.$axios = axios`

http://115.29.141.32:8085/#/mall/show/index

http://115.29.141.32:8084/api/mall/getGoodsByType?typeId=-1

```js
this.$axios.get('http://115.29.141.32:8084/api/mall/getGoodsByType?typeId=-1')
        .then(res => {
            console.log(res)
    		//在dom树的根节点中，定义一个对象来接收服务器传来的数据
            _this.obj = res.data.data
        })
        .catch(error =>{
            console.log(error)
        })
}
```

## 2.2 补充

### 2.2.1 前端

前端：组件化的天下

### 2.2.2 Element-ui



![image-20200617082756587](C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200617082756587.png)

### 2.2.3 Iconfont



![image-20200617082831466](C:\Users\飒飒\AppData\Roaming\Typora\typora-user-images\image-20200617082831466.png)

### 2.2.4 补充

http://115.29.141.32:8085/#/mall/show/index

https://iview.github.io/

https://www.iconfont.cn/

http://www.googlefonts.cn/

https://v-charts.js.org/#/funnel

https://element.eleme.cn/#/zh-CN/component/layout