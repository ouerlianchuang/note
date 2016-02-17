# js设计模式 笔记

标签： js 笔记

---

> 写出可维护的代码

### 设计模式
一个模式就是一种可重用的方案，是不断对代码优化重构的合理解。
不同的设计模式是针对不同的问题需求的解决方案，不会有一种设计模式适合解决所有问题。

自己的锅⬇️

+ 定义大量污染全局的变量
+ 过长的的嵌套的函数
+ 不合理的随意的命名
+ 重复的无效率的片段
+ 随意修改覆盖的原型的方法
+ 越界功能的额外调用的函数方法
+ 耦合的牵扯其它部分状态改动的

---

最近看js设计模式和最佳实践到第二遍， 再去review代码，的确觉得代码有了不少新问题，自以为确实和以前的看法有了些不同的切入点，可能是和最近正好在写vue的笔记有关，个站也开始用vue写了，吸收了些vue的设计思路，除了上面列表里的错误点外，对从开始写项目之初就要对代码整体有所设计有了新的认识。
从最初的线式写代码，想到哪写到哪，想加什么打补丁，东调西，西挪北，到条式的逐条功能性添加，按需写函数，调用偶尔串个门，到有点模块性思维，考虑重用和维护性，向往规范和优雅，再到组件化和应用整体考量，考虑抽象功能和属性，vue本地状态和应用状态单向数据的思路貌似让我有点成长。







——————————————
之前看read课时的 review
```
文件中有多个全局变量，多个函数中的状态受在这个函数中看不到的方法操作影响，那这样，我们把read课或者整个lessons看成class，我们有重复的 pre2cm Katex ProgressBar操作有复写的has_saved操作,上下页的具体操作可以改写成对read方法的调用，去改变页码状态更新内容,类似
class read
     constructor: (num,paragraphs...)->
     setnum: ->
        @name--++
        setStatus(num)
     setStatus:->
        修改段落（num）
        修改进度（num）
     修改段落：->
      // 修改....
      完成（）
     ...

点击上下页 ->
    readSetNum--++
```
————————————
后来看teach域模块的review
```
teach域下的右侧页面虽然有不同的类型，也有雷同的迥异的操作，但是基于add和update我们可以把右侧看作一个整体或组件，课程类型的改变只是零件加入或切换，最后的行为或者操作目的是相同的，post的url和参数可以看作固有可复写的属性，所有课程中的title和tag,id也是可以固定获取的属性，error完全一致不用说，done的操作也是可以拆分成两个可复写方法

class TachLesssonsManage
     constructor: (data,addUrl,upUrl)->
     init: (data)->
          defaults=  {
              title,tags,course_id,course_chapter_id。。。。
          }
         @data = $.extend({},defaults,data)
      editor:->
          .......
      donesuccess:()->
          return 'false'
      doneerror:(msg)->
          alertinfo  msg
      error:()->
          .......
      post:(url)->
          $.post addUrl/upUrl,
              data
          .done ->
              .....


又想想，这样写下来，若果read，code之类的课去extend TachLesssonsManage，好像又把想要将右侧看作一个完整组件的想法破坏了，我们还是会像下面这样render的方法分别处理

pagePartRender = ->
      sharedRender()
      switch page
            when 'chapter'
                Chapter.render()
            when 'match'
                Match.render()
            when 'read'
                Read.render()
            when 'choice'
                Choice.render()
            when 'code'
                Code.render()
            when 'challenge'
                Challenge.render()

所以，我们其实可以把php传来的page看作TachLesssonsManage的状态
我们把右侧的所有看成整体，我们的操作不是在切换课程类型切换页面，我们通过在TachLesssonsManage中设置state，然后在course_manage.coffe页面的render（）部分就可以修改成

new TachLesssonsManage().setState(page)

然后我们的TachLesssonsManage 就需要对应不同课程类型搭配state修改
但是，url，done之类我们可以按照state配对放进 { } ,不同课程需要要的不同方法，match文件，file修改之类的放在外面就可以反正只要exports TachLesssonsManage，每个页面的事件怎么办啊，那这样想donesuccess的函数也不能堆列进TachLesssonsManage里，soga，只能按整state名封装在另外的函数里，codeInitEvent(),readInitEvent()...要做的

TachLesssonsInitEvent = {
   code: ()->,
   read: ()->
   ...
}
//这样只要setSate()时对应调用就好了 doneSuccess也可以
```
