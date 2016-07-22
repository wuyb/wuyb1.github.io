---
layout: post
title:  "React Native 和 Redux 初体验"
date:   2016-07-22 19:43:10
categories: 技术, 前端, React Native, Redux
---

最近用业余时间撸了一个 iOS + Android 的玩具，后端用了 Spring Boot （这个后面有机会也会写一下），而移动端则尝试使用了 React Native 搭配 Redux。

大概用了一周的时间搭起来一个完整的框架，感觉学习曲线很陡。原因很简单，不论是 Cocoa Touch 还是 Android，都是同样的 MVC 模式。而 React + Redux 则在架构上就是全新的。最根本的区别在于任何东西都是不可变的（Immutable）。Redux 通过 Action + Reducer + Store 的数据流逻辑保证了这一点。

React Native 和 Redux 是完全独立的两个东西，只是因为二者对于数据的不可变性要求是统一的，所以他们非常适合在一起。

** React 和 React Native **

React 和 React Native 本质上是对底层渲染体系（DOM, UIView, Activity）的二次封装。但是在此基础上提出了一个非常重要的理念：只能有一种方式去修改页面，即 `setState()`。这样的好处是数据流非常清晰，并且更重要的是，React 可以通过对比数据（props）的变化，来决定页面的哪一部分需要重新渲染（这个则是通过 React 的组件化思维来实现的，即页面上的每个组件独立决定自己是否需要被重新渲染，而不是整个页面。参见 `render()` 函数的定义）。这样一来，性能就有了保证。对比其他前端框架（其实 React 算不算框架不好说），JQuery 就不必多说了，现在还裸写 JQuery 的一定是铁粉。Angular 在一定程度上解决了 JQuery 的主要问题，开发效率也非常高。但是 Angular 的渲染方式仍然依赖 DOM 操作，虽然有绑定，但是绑定的太多，由于它的 Dirty Checking 机制，一点点变化都会触发 Dirty Checking。更重要的是，Angular 还是太灵活，Coder 很容易滥用其中的一些灵活性，而带来更多的问题。
（所谓 Dirty Checking，就是去遍历 Model 来判断哪些数据有了更新，然后去刷新绑定的页面元素）

** Redux **

Redux 可以说是 Flux 的一种实现。它的理念非常简单：数据单向流动 + 一切都非可变。简单来说，Redux 的工作流程是这样的：

1. Action 是一切变化的源头。不管是页面操作还是网络请求返回，需要对页面有更新，都要通过 Action。
2. Action 通过 Reducer 产生数据的变化，这个变化是在原有数据的拷贝上进行，而不是直接操作。Reducer 只产生新数据，不能产生任何其他的副作用，防止改变或中断数据流的方向。
3. 新的数据拷贝进入 store，store 会提醒所有 Subscriber 来响应变化。

而对于 React + Redux, 可以通过 react-redux 库里的 `connect()` 函数来将 store 产生的新数据转化为 React 组件的数据，同时，这个函数会将 `dispatch()` 函数注入到 React 组件里，那么 React 就可以用它来发布 Action 了。

大概就是这个样子，其实概念都非常简单。之所以说它学习成本高，是因为第一文档实在是太简单了，第二要改变原来的思想还是需要个时间。所以我这个玩具其实到现在也只是实现了用户注册的功能而已。另外就是 React Native 的 API 还比较简单，要实现一些基本功能都要绕一段路。比如说要实现某些 Scene 的 Navigation Bar 隐藏，就需要在页面显示前发一个 Action 表示隐藏还是显示，然后 Reducer-Store 过一遭，Scene 里面 `mapStateToProp` 将 Redux 数据（state）转变为 React 数据（props）。虽然很复杂，然而这么写完发现，真的还是很优雅的一种解决方案。

空口无凭，有时间我会把代码放出来的。
