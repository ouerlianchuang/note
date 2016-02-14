
# React 分享

标签： react 演讲参稿

---

> 概念： 组件 | jsx | Virtual DOM(DOM diff) | Data Flow

<b />

    React 的核心思想是：封装组件。 1，2，3，4，5...
    然后各个组件维护自己的状态和UI，当状态变更，自动重新渲染整个组件。
    基于这种方式的一个直观感受就是我们不再需要不厌其烦地来回查找某个 DOM 元素，然后操作 DOM 去更改 UI。(flux)

    //总结

    + 组件化的思想深入人心
    + React通过virtual-dom和dom-diff的技术，避免了频繁操作DOM带来的性能损耗，开发的应用很流畅；
    + React通过virtual-dom实现了同构JS，这样一来前后端可以使用一套模板，节省了传统开发模式中要在前后端两套模板的时间；
<b />
## react react-dom
0.14 _> react 被拆分为 react和react－dom
//react-dom/server  .renderToString 和.renderToStaticMarkup(不带react－data－id之类的属性) 字面解 获得更快的页面加载速度，并且有利于搜索引擎抓取页面，方便做 SEO 如果在一个节点上面调用 React.render()，并且该节点已经有了服务器渲染的标记，React 将会维护该节点，并且仅绑定事件处理器，保证有一个高效的首屏加载体验。
### 顶层api
+ react (API)
    - React.createElement  // 创建一个指定类型的reactelement ，（app.coffee）  Virtual DOM
    - React.createClass // 创建一个组件类，并作出定义，render()方法 返回一个子级（看hello world）
    - Component //使用 ES6+ 来编写 React 组件最明显的变化就是我们定义组件（类）的语法的方式。我们可以用定义一个继承了 React.Component 的ES6 类来代替原本使用 React.createClass 的来创建类的方式：
    - .PropTypes // 包含了能与组件 propTypes 对象共用的类型，用于验证传入组件的 props
    - .Children // 提供 this.prorps.children数据结构 的工具 map锕foreach啊之类

+ react-dom
    - ReactDom.render //渲染一个reactelement到dom中
    - .unmountComponentAtNode // 移除已挂在的组件， 清除相关事件和state
    - .findDOMNode
+ Add-ons

<b />
<b />

### 组件api

+ setState
+ replaceState
+ setProps
+ replaceProps
+ isMounted()
+ getDOMNode

## 关键字（实现）
<b />
  >分离 ： 状态管理 ｜ 渲染逻辑

    目的：   构建可复用的组件构建用户界面（数据交互繁重 ），自动管理ui更新（dom渲染重）
    组件－－》有限状态机 （描述）
    （props，state 核心 一个组件正式通过这两个属性的值生成组件对应的html结构）
    React 把用户界面当作简单状态机。把用户界面想像成拥有不同状态然后渲染这些状态，可以轻松让用户界面和数据保持一致。

React 里，只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。React 来决定如何最高效地更新 DOM
    生命周期：规定了组件的状态和方法需要在那个阶段进行改和执行

  >有限状态机（FSM），表示有限个状态以及在这些状态之间的转移和动作等行为的模型。一般通过状态、事件、转换和动作来描述有限状态机。状态机，能够记住目前所处的状态，根据当前的状态可以做出相应的决策，并且在进入不同的状态时，可以做不同的操作。通过状态机将复杂的关系简单化，利用这种自然而直观的方式可以让代码更容易理解。

    React 正是利用这一概念，通过管理状态来实现对组件的管理。例如，某个组件有显示和隐藏两个状态，通常会设计两个方法 show() 和 hide() 来实现切换；而 React 只需要设置状态 setState({ showed: true/false }) 即可实现（状态管理）。（具体去管－》）同时，React 还引入了组件的生命周期概念。通过它就可以实现组件的状态机控制，从而达到 “生命周期－状态－组件” 的和谐画面。
（demo）生命周期
## 生命周期

+ 装载
    + componentWillMount
    + componentDidMount
+ 更新
    + componentWillReceiveProps //在组件接收到新的 props 的时候调用。在初始化渲染的时候，该方法不会调用。
    + shouldComponentUpdate //在接收到新的 props 或者 state，将要渲染之前调用。该方法在初始化渲染的时候不会调用，在使用 forceUpdate 方法的时候也不会
    + componentWillUpdate
    + componentDidUpdate
+ 卸载
    + componentWillUnmount


+ 当首次装载组件时，按顺序执行 getDefaultProps、getInitialState、componentWillMount、render 和 componentDidMount；
+ 当卸载组件时，执行 componentWillUnmount；
+ 当重新装载组件时，此时按顺序执行 getInitialState、componentWillMount、render 和 componentDidMount，但并不执行 getDefaultProps；
+ 当再次渲染组件时，组件接受到更新状态，此时按顺序执行 componentWillReceiveProps、shouldComponentUpdate、componentWillUpdate、render 和 componentDidUpdate。

### Component Lifecycle
自定义组件（ReactCompositeComponent）的生命周期主要通过三种状态进行管理
    MOUNTING、RECEIVE_PROPS、UNMOUNTING，
它们负责通知组件当前所处的状态，应该执行生命周期中的哪个步骤，是否可以更新 state。
三个状态对应三种方法，分别为：mountComponent、updateComponent、unmountComponent，每个方法都提供了两种处理方法，will 方法在进入状态之前调用，did 方法在进入状态之后调用，三种状态三种方法五种处理方法，此外还提供两种特殊状态的处理方法。


mountComponent 负责管理生命周期中的 getInitialState、componentWillMount、render 和 componentDidMount。
![s](http://ww4.sinaimg.cn/mw690/61ff3868gw1exj76ac37ej20cb0csdh8.jpg)
updateComponent 负责管理生命周期中的 componentWillReceiveProps、shouldComponentUpdate、componentWillUpdate、render 和 componentDidUpdate。
![s](http://ww4.sinaimg.cn/mw690/61ff3868gw1exj7lmihhwj20ct0cytah.jpg)
unmountComponent 负责管理生命周期中的 componentWillUnmount。


##  Virtual DOM 与 diff


React 中最值得称道的部分莫过于 Virtual DOM 与 diff 的完美结合，特别是其高效的 diff 算法，让用户可以无需顾忌性能问题而”任性自由”的刷新页面，让开发者也可以无需关心 Virtual DOM 背后的运作原理，因为 React diff 会帮助我们计算出 Virtual DOM 中真正变化的部分，并只针对该部分进行实际 DOM 操作，而非重新渲染整个页面，从而保证了每次操作更新后页面的高效渲染，因此 Virtual DOM 与 diff 是保证 React 性能口碑的幕后推手。

+ 用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中
+ 当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异
+ 把2所记录的差异应用到步骤1所构建的真正的DOM树上，视图就更新了

## 事件 （React合成事件）
> react自己实现了一套事件体系，事件模型保证了和 W3C 标准保持一致，所以不用担心有什么诡异的用法，并且这个事件层消除了 IE 与 W3C 标准实现之间的兼容问题。 (屏蔽了事件差异)

<b />
onchange(原生)
input失去焦点时触发
希望：文字输入，复制粘贴进去，撤销，右键 触发
react 抹平了这些差异

dispatchEvent (每次都会掉用这个函数)

<b>
划分状态数据
一条原则：让组件尽可能地少状态。
这样组件逻辑就越容易维护。
什么样的数据属性可以当作状态？
当更改这个状态（数据）需要更新组件 UI 的就可以认为是 state，下面这些可以认为不是状态：
可计算的数据：比如一个数组的长度
和 props 重复的数据：除非这个数据是要做变更的

 + 原始产品数组
 + 用户输入的关键字
 + 复选框状态
 + 过滤后的产品数组

// 他是从父级传过来的吗，他会经常改变吗，他可以被其它state和props计算出来吗


// state 应该属于哪个组件？？ 1.找到所有需要用到这个state的组件，2.找到他们的公共祖先组件3.此组件活着他的祖先组件应该拥有这个state4.如果没有合适的，可以创建一个新的组件，它包含所有又到这个state的组件




<b></b>


> 更新数据源，（setState）  一层层传递，更新，（优化了dom节点复用，无需关心绘制效率，有接口可以手动优化这些细节，fb：你们大可不必这样）

<b>

// Data Flow 只是一种应用架构的方式，比如数据如何存放，如何更改数据，如何通知数据更改等等，所以它不是 React 提供的额外的什么新功能，可以看成是使用 React 构建大型应用的一种最佳实践。(Flux, Redux)

## JSX

为什么要引入 JSX 这种语法

传统的 MVC 是将模板放在其他地方，比如 <script> 标签或者模板文件，再在 JS 中通过某种手段引用模板。按照这种思路，想想多少次我们面对四处分散的模板片段不知所措？纠结模板引擎，纠结模板存放位置，纠结如何引用模板……下面是一段 React 官方的看法：
>We strongly believe that components are the right way to separate concerns rather than "templates" and "display logic." We think that markup and the code that generates it are intimately tied together. Additionally, display logic is often very complex and using template languages to express it becomes cumbersome.

<b>
> 我们始终坚信，组件使用了正确方法去做关注分离，而不是通过“模板引擎”和“展示逻辑”。我们认为标签和生成它的代码是紧密相连的。此外，展示逻辑通常是很复杂的，通过模板语言实现这些逻辑会产生大量代码。
我们得出解决这个问题最好的方案是通过 JavaScript 直接生成模板，这样你就可以用一个真正语言的所有表达能力去构建用户界面。为了使这变得更简单，我们做了一个非常简单、可选类似 HTML 语法 ，通过函数调用即可生成模板的编译器，称为 JSX。
你不需要为了 React 使用 JSX，可以直接使用纯粹的 JS。但我们更建议使用 JSX , 因为它能定义简洁且我们熟知的包含属性的树状结构语法。


```
var child = React.createElement('li', null, 'Text Content');
var root = React.createElement('ul', { className: 'my-list' }, child);
React.render(root, document.body);
```

<b>

<b>
简单来说，React 认为组件才是王道，而组件是和模板紧密关联的，组件模板和组件逻辑分离让问题复杂化了。显而易见的道理，关键是怎么做？
所以就有了 JSX 这种语法，就是为了把 HTML 模板直接嵌入到 JS 代码里面，这样就做到了模板和组件关联，但是 JS 不支持这种包含 HTML 的语法，所以需要通过工具将 JSX 编译输出成 JS 代码才能使用。


http://segmentfault.com/a/1190000004003055

https://github.com/my-fe/wiki/issues/1?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io
