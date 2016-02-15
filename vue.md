# Vue 笔记

标签： vue js 笔记

---
> 组件化干嘛 官方：可以利于多人开发，便于扩展，当然也可以提高代码的可读性与可维护性。
为了提升开发效率啊，加快开发速度 减少变更代价 方便替换

---

> 干嘛要单页应用啊，单页应用有什么好啊。。


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

### 生命周期
生命周期既是vue实例化的步骤（比如建立观察，编译模版数据绑定什么的），与此同时实例会调用一些勾子函数，给码农自定义行为的机会。

```
var vn = new Vue({
    data: {
        a: 1
    },
    created: function (){
        console.log(this.a)
    }
})
// 以上的 created就是个勾子函数，会在vue实例的生命周期处于创建后状态同步调用
```
vue实例的生命周期从 new Vue()执行开始，首先触发created(),这个时候选项的解析完成，建立了数据绑定，属性计算啊啊，方法啊，但是dom还没开始编译，所以这个时候你在created（）中调用`$el`是无效的，created（）后就开始编译啦，编译dom当然要先看有el选项否，有 接着看 是否有template，这个时候触发beforeCompile()（编译dom节点前），接着开始编译，有模版就编译模版替换掉文档中的el，没有就算el的，pia,编译完成，周期勾子走到compile（），然后插入到document，ready（）勾子触发，剩下两个属于死亡周期了，destroy（）beforeDestroy()，一个销毁实例开始时，一个销毁后调用(使用这个vm.$destroy()的时候触发，可以传参数true，dom级删除，这个时候会出发datached（）勾子)。

### v-bind v-on
```
//v-bind  响应更新html特性
<a v-bind:href="url"></a>
<a :class="active"></a> //缩写
//v-on 监听dom事件
<a v-on:click="todo"></a>
<a @:click="todo"></a>
```
```
// class的动态切换
<a :class="{classa : isa, classb : isb}">
<a :class="[classa, isB ? 'classb' : '']">
// :style css属性会自动加前缀

<li class="list-group-item"
    v-for="(index,list) in lists"
    v-bind:class="[list.active ? 'active' : '']"
    v-on:click="setListActive(index)">
    {{ list.content }}
</li>
```
### 计算属性 computed

```
<div id="demo1"></div>
```
```
var vm = new vue({
    data: {
        a: 1
    },
    computed: {
        b: function () {
            return this.a + 1
        }
    }
})
```
这里  b同样是vm的属性，它会因a的改变而改变，b依赖于a

计算属性 默认只有getter只有获取权限，你也可以设置setter权限
```
computed:  {
    b: {
        get: function() {
            return a + 1
        },
        set: function(v) {
            this.a = v - 1
        }
    }

}
```
### 指令
指令，带特殊前缀的HTML特性，可以让 Vue.js 对一个 DOM 元素做各种处理
v-text 更新元素的textContent 相当于 ｛｛text｝｝
v-html 更新innerhtml
v-if v-else v-show
```
<a v-if="true">show</a>
<a v-else>hide</a>

<template v-if="true">
    innerhTML
</template>

// if 与 show的区别在于if会判断是否出现在文档流，show是单纯的display切换
```
v-for
```
<ul>
    <li v-for="(index,list) in lists"
        :class="list.active"
        @click="click(index)">
        list.content
    </li>
</ul>
```
on bind
`上面⬆`️
v-on 绑定事件监听器 只见听原生dom的
子组件上使用，可以绑定自定义事件
```
<my-component @my-event="todo"></my-component>


// v-on可以使用事件修饰符 类似于原生中的
event.preventDefault() event.stopPropagation()
@click.stop="dosom"阻止事件冒泡
@submit.prevent="onsubmit"阻止默认事件
```

```
// 其它还有按键修饰符
v-on:keyup.13="submit"
// 别名 entenr tab delete esc space up down lef right
@keyup.entenr="submit"
```
v-model
在表单控件元素上创建双向数据绑定

```
<span>Msg is {{ msg }}</span>
<input type="text" v-model="msg" placeholder="edit me"></input>
//span 的显示会响应用户对input的输入
```

v-ref
```
//可以让你直接在js中访问子组件 react中类似
<div id="parent">
    <mychildcomponent v-ref:mcc></mcc>
</div>

<script>
    var parent = new ({el= "#parent"})
    var child = parent.$refs.mcc
    //当和for一起使用时 mcc是个对象或数组
</script>
```
v-el
类似v-ref这个是为dom创建索引
v-pre
跳过当前节点的编译
cloak

##### 自定义指令
我们可以使用Vue.directive(id,definition)方法注册一个全局的自定义指令，参数一个是指令id如 el啊if啊show啊
definition是‘定义对象’用来定义指令的勾子函数
包括bind update unbind
```
// 勾子内this指向 指令对象 指令对象暴露了部分属性
Vue.directive('my-directive',{
    deep: true, // 当指令用在一个对象上时 对象内部变化触发update
    twoWay: true, // 允许指令中使用 this.set(value)，向实例写回数据
    acceptStatement: true, // 允许指令接受内联语句
    priority: 100,// 指定优先级 顺次处理但是v-if v-for有最高优先级
    bind: function() {
    //做绑定工作监听事件之类的或者是指执行一次的复杂事件
    },
    update: function(newvalue,oldvalue) {
    // <div v-my-directive:hello.a.b="msg"></div>
    // this.el 指绑定的元素
    // this.vm 拥有该指令的上下文viewmodel
    // expression 指令表达式（未被解析的） 不包括 参数和过滤器 msg
    // arg 指令参数 hello
    // name 指令名字 不包含前缀 my-directive
    // modifiers 一个对象包含指令修饰符  a.b
    // descriptor 一个对象指令的解析结果
    // nv msg
    },
    unbond: function(){
    //清理工作 比如解绑事件
    }
})
```

### 过滤器
Filters是过滤器用于在更新视图之前处理原始值的函数。它们通过一个“管道”,在指令或绑定中进行处理：
```
<div>{{message | capitalize}}</div>
```
这样在 div 的文本内容被更新之前，message 的值会先传给 capitalizie 函数处理。)
vue已有的⬇

+ capitalize // ｛｛ msg | capitalize ｝｝ abc -> Abc
+ uppercae // abc -> ABC
+ lowercase // ABC -> abc
+ currency // {{ num | currency '¥'}} 1234 -> ¥1,234.00默认$
+ pluralize // {{ count | pluralize 'st' 'nd' 'rd'}} 1 -> 1st 2->2nd 3->3rd 4->4rd
+ json // JSON.stringify() 对象转字符串
+ debounce // 包装处理器，让它延迟执行 x ms <input @keyup="onKeyup | debounce 500"> 限定指令值为函数
+ limitBy // 数组限制开始和偏移
+ filterBy // 数组过滤
+ orderBy // 排序

##### 自定义过滤器
使用Vue.filter(id,fn)注册,参数为过滤器id和过滤器函数
```
//
Vue.filter('reverse', function(value,before,after){
    return before + value.split('').reverse().join()
})

Vue.filter('currencyDisplay', {
  // model -> view
  // 在更新 `<input>` 元素之前格式化值
  read: function(val) {
    return '$'+val.toFixed(2)
  },
  // view -> model
  // 在写回数据之前格式化值
  write: function(val, oldVal) {
    var number = +val.replace(/[^\d.]/g, '')
    return isNaN(number) ? 0 : parseFloat(number.toFixed(2))
  }
})
```

### 组件
在vue中我们可以把Vue扩展出来的viewmodel子类当作可服用的组件，只需要调用 Vue.extend()创建一个组件构造器

```
 var MyChild = Vue.extend({
      //options
  })
  var MyParent = Vue.extend({
    template: '<p>balabala</p>',
    data: function() {
        return {a:1}
    },
    components: {
        'my-child-component': MyChild
    },

  })

  //组件可以 用 Vue.component('my-parent',MyParent)全局注册 也可以用选项components局部注册在组件内
```

##### 通信&&传递

组件之间的作用域是孤立的
我们不能在子组件里直接调用父组件的数据，我们可以使用props把数据传递给子组件
prop是组件options的一个字段期望从父组件传递过来
```
var MyChild = Vue.extend({
    prop: ['msg'],
    template: '<p>{{ msg }}</p>'
})
var MyParent = Vue.extend({
    components: {
        "my-child": MyChild
    },
    template: "<div><my-child msg="hello"></my-child>111</div>"
    //动态props
    //template: "<div><my-child :msg.sync="parent-data"></my-child></div>"
})

// <div><p>hello</p>111</div>
// props的传递默认是单向绑定 只有父组件的属性变化时 才会传递给子组件，反过来是不会的。这是为了防止子组件的变化无意改变了父组件的状态，如果 prop 是一个对象或数组，是按引用传递。在子组件内修改它会影响父组件的状态，不管是使用哪种绑定类型。 想起面向对象中类的继承中extend的实现，中间为了防止子类原型的修改影响到基类也做了处理。
```
prop的值是可以在组件中认证的
```
props: {
    propa: NUmber,
    propb: {
        type: String,
        default: 'aaa'
    },
    .....
}
```
子组件中可以用 `this.$parent`访问父组件，父组件也有一个`this.$children`

尽管我们可以相互访问父子，但是我们还是应该尽可能的避免直接对与其他层级数据的依赖.这会避免父子组件间的耦合，同时如果我们只去看父组件，很难全面了解父组件的状态，因为它可能被子组件影响修改，理想情况，组件状态只有自身可以修改。

vue的实例拥有一些自定义的事件接口，用于在组件树中通信。

+ `$on`监听事件
+ `$emit` 触发
+ `$dispatch` 派发事件 事件沿着父链冒泡
+ `$broadcasr` 广播事件 父->子

```
var MyChild = Vue.extend ({
    parent: vmParent,
    methods: {
        evevtxx: function(){
            this.$disoatch('child-msg', 'abc')
        }
    }
})

var vmParent = new Vue({
    ...
    events: {
        child-msg: function() {
            ...
        }
    }
})

// 也可以 ⬇
<child v-on:child-msg="handleIt"></child>
```
##### sloat

### 过渡

vue中的过渡指的是元素在dom中插入或者移除触发的效果

```
// v-if v-show v-for 动态组件 $appendto()
<div v-if="show" transition="show"></div>
// 当插入或删除时 先去找js过渡勾子
Vue.transition('show',{
    beforeEnter: function (el) {
        el.textContent = 'beforeEnter'
    },
    enter: function (el) {
        el.textContent = 'enter'
    },
    afterEnter: function (el) {
        el.textContent = 'afterEnter'
    },
    enterCancelled: function (el) {
        // handle cancellation
    },
    beforeLeave: function (el) {
        el.textContent = 'beforeLeave'
    },
    leave: function (el) {
        el.textContent = 'leave'
    },
    afterLeave: function (el) {
        el.textContent = 'afterLeave'
    },
    leaveCancelled: function (el) {
        // handle cancellation
    }
})
// 然后找css
.show-transition {
    这个css会始终保留在元素上
}
.show-enter 过度开始时的状态
.show-leave 过度结束时的状态
```
### 其它API
