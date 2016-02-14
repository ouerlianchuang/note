# Vue 笔记

标签： vue js 笔记

---

Vue 和angular一样都是mvvm框架
什么是mvvm框架呢
我们知道mvc框架指的是软件或者着应用可以分为view.controller.model,即视图控制器模型，或者说是用户界面业务逻辑和数据保存，三个区块间是单项通信，即v传指令到c，c完成逻辑，要求m改变状态，m将新数据发送到v，v改变。（也可以直接对c下指令）
mvvm框架即model，view和viewmodel，是mvc的衍生物，关注点在于将页面和数据逻辑分离，减少dom操作，操作数据即可。视图和试图模型之间通过数据绑定和事件进行通讯，视图只处理自己的用户接口事件，然后把相关事件映射到试图模型，我们通过双向数据绑定进行更新，（没弄清楚啊，再来）
mvvm中的model代表我们的相关数据和信息，但它只持有信息，没有操作行为，他不会格式化信息，也不会影响数据在浏览器中的表现。view是用户唯一打交道的部分，它是展现试图模型状态的一个可交互的ui（重要的是它与viewmodel同步）。viewmodel可以看作view的提供者或者掌控者，也不太对，隔着model说是个中间提供掌控者把，它可以为view提供数据和方法，怎么提供呢，我们把viewmodel看作对象，有自己的属性和方法，这些属性会通过转换展示在view，view就是用来展示试图模型状态的，不同状态属性不一，就改变了视图，而视图是与viewmodel交互的ui，所以，我们定义了最初的vm状态，初始显示页面了，用户操作ui了改变视图模型状态了，影响对应的属性了视图更新改变了。
还是有点懵懵的，但是了解点mv＊后再去看vue介绍就清楚点了。

vue.js是mvvm框架，那么页面和数据逻辑分离，减少dom操作必然是他所要实现的。

```
// 下面的代码就是创建一个vue实例，
// 每一个vue实例都是一个viewmodel
var vm = new vue({
    el: '#demo-1',
    data: {
        one:'world',
    }
})
```

```
<!-- 这里是view -->
<div id="demo-1">
    Hello {{ one }}
</div>
```
以上的代码会在页面中显示 Hello world。
通过上面mvvm的介绍可以看的到view基于viewmodel的数据做了对应显示，同样的如果我们在控制台修改了vm.$data.one='vue'，页面中就会显示Hello vue。

在vue中每一个vue实例会创建一个viewmodel，而视图view则是vue实例管理的dom节点，上面的代码中就是‘#dem-1’，即`vm.$el`。在vue中每当一个实例被创建时，vue会递归遍历跟元素的所有子节点，同时完成数据绑定，当视图被编译后，他就会自动响应数据的变化了，作为码农几乎是不需要操作dom的，当数据变化时，视图会自动更新（批量异步）。
model，vue中的模型就是js对象，当它作为实例的数据后，你可以操作它的属性，同时观察它的vue会收到提示。
（每个vue实例都会代理data里的属性，你可以vm.one＝‘vue’。使用`$data`是vue实例暴露了部分实例属性和方法 不过要统一加$。vue的观察不会响应新添加或者删除的属性）

vue.js的官方描述里说
> Vue.js是一个构建数据驱动的 web 界面的库。Vue.js的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。
Vue.js 专注于 MVVM 模型的 ViewModel 层。它通过双向数据绑定把 View 层和 Model 层连接了起来。实际的 DOM 封装和输出格式都被抽象为了 Directives 和 Filters。

(Directives是指令，带特殊前缀的HTML特性，可以让 Vue.js 对一个 DOM 元素做各种处理。

Filters是过滤器用于在更新视图之前处理原始值的函数。它们通过一个“管道”,在指令或绑定中进行处理：
```
<div>{{message | capitalize}}</div>
```
这样在 div 的文本内容被更新之前，message 的值会先传给 capitalizie 函数处理。)


