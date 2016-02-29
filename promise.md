# Promise

标签：笔记
---
(each和map的区别，map返回一个新的数组，主要便利属组合对象，each主要遍历jquery对象，返回原数组)

> 这篇笔记的目的是帮助我了解promise的实现，主要纪录我在看《JavaScript Promise迷你书》和部分相关博客时的想法和随笔。

Promise是异步编程的解决方案，js2015将其写入了语言标准，原生提供了promise对象。

说promise前我们先来聊下js的观察者模式，也叫发布－订阅模式。观察者模式定义了一种一对多的依赖关系，当一个对象的状态改变时，所有依赖他的模块都会收到通知，在js开发中，我们一般使用事件模型替代传统的观察者模式。jquery中的deferred就是一种观察者模式的实现，是回调函数的解决方案。比如1.5.0版本后的jquery中ajax的回调函数可以直接在屁股后`.done().success().file()`之类的，因为它返回的是一个deferred对象。
```
// 定义deferred对象

var subject = (function(){
    var dfd = $.Deferred;
    var task = function () {
        // 发布内容
        dfd.resolve('content');
    };
    setTimeout(task, 200);
    return dfd;
})()

// 观察者
var fn1 = function (content) {}
var fn2 = function content() {}

// 注册观察者
$.when(subject)
.done(fn1)
.done(fn2)

```
(dai.xu)








