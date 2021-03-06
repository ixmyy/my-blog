---
title: nodejs入门（一）
date: 2018-12-06 16:00:05
tags: 
    - nodejs
categories: javascript
toc: true
---

本文主要记录nodejs入门相关知识

<!-- more -->
## nodejs是用来做什么的
Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。

Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。(事件驱动：事件触发过程中，进行决策的一种策略，简单说就是跟随当前时间点上出现的事物，调用可用的资源进行解决该事物，使得不断出现的事物得以解决，防止事物堆积)

Node.js 的包管理器 npm，成为世界上最大的开放源代码的生态系统。

那么：


“为什么我们要用node.js？”，毫无疑问：它有几个特别显著的优点：**快，性能高，开发效率高，应用范围广**
别人一般会含糊的告诉你：node.js有非阻塞，事件驱动I/O等特性，从而让高并发（high concurrency）的轮询（Polling）和comet构建的应用中成为可能。
当你看完这些解释觉得还是不太理解的时候，下面我就简单粗暴的来帮助你你理解理解node.js。


浏览器给网站发请求的过程一直没怎么变过。当浏览器给网站发了请求。服务器收到了请求，然后开始搜寻被请求的资源。如果有需要，服务器还会查询一下数据库，最后把响应结果传回浏览器。

不过，在传统的web服务器中（比如Apache），每一个请求都会让服务器创建一个新的进程来处理这个请求。

后来有了Ajax。有了Ajax，我们就不用每次都请求一个完整的新页面了，取而代之的是，每次只请求需要的部分页面信息就可以了。这显然是一个进步

。但是比如你要建一个FriendFeed这样的社交网站（类似人人网那样的刷朋友新鲜事的网站），你的好友会随时的推送新的状态，然后你的新鲜事会实时自动刷新。

要达成这个需求，我们需要让用户一直与服务器保持一个有效连接。目前最简单的实现方法，就是让用户和服务器之间保持长轮询（long polling）。

HTTP请求不是持续的连接，你请求一次，服务器响应一次，然后就完了。长轮询是一种利用HTTP模拟持续连接的技巧。具体来说，只要页面载入了，不管你需不需要服务器给你响应信息，你都会给服务器发一个Ajax请求。

这个请求不同于一般的Ajax请求，服务器不会直接给你返回信息，而是它要等着，直到服务器觉得该给你发信息了，它才会响应。比如，你的好友发了一条新鲜事，服务器就会把这个新鲜事当做响应发给你的浏览器，然后你的浏览器就刷新页面了。浏览器收到响应刷新完之后，再发送一条新的请求给服务器，这个请求依然不会立即被响应。于是就开始重复以上步骤。利用这个方法，可以让浏览器始终保持等待响应的状态。虽然以上过程依然只有非持续的Http参与，但是我们模拟出了一个看似持续的连接状态


我们再看传统的服务器（比如Apache）。每次一个新用户连到你的网站上，你的服务器就得开一个连接。每个连接都需要占一个进程，这些进程大部分时间都是闲着的（比如等着你好友发新鲜事，等好友发完才给用户响应信息。或者等着数据库返回查询结果什么的）。

虽然这些进程闲着，但是照样占用内存。这意味着，如果用户连接数的增长到一定规模，你服务器没准就要耗光内存直接瘫了。

这种情况怎么解决？解决方法就是刚才上边说的：**非阻塞和事件驱动**。这些概念在我们谈的这个情景里面其实没那么难理解。

你把非阻塞的服务器想象成一个loop循环，这个loop会一直跑下去。一个新请求来了，这个loop就接了这个请求，把这个请求传给其他的进程（比如传给一个搞数据库查询的进程），然后响应一个回调（callback）。完事了这loop就接着跑，接其他的请求。这样下来。服务器就不会像之前那样傻等着数据库返回结果了。

如果数据库把结果返回来了，loop就把结果传回用户的浏览器，接着继续跑。在这种方式下，你的服务器的进程就不会闲着等着。从而在理论上说，同一时刻的数据库查询数量，以及用户的请求数量就没有限制了。服务器只在用户那边有事件发生的时候才响应，这就是事件驱动。

FriendFeed是用基于Python的非阻塞框架Tornado (知乎也用了这个框架) 来实现上面说的新鲜事功能的。不过，Node.js就比前者更妙了。

Node.js的应用是通过javascript开发的，然后直接在Google的变态V8引擎上跑。用了Node.js，你就不用担心用户端的请求会在服务器里跑了一段能够造成阻塞的代码了。因为javascript本身就是事件驱动的脚本语言。你回想一下，在给前端写javascript的时候，更多时候你都是在搞事件处理和回调函数。javascript本身就是给事件处理量身定制的语言。


Node.js还是处于初期阶段。如果你想开发一个基于Node.js的应用，你应该会需要写一些很底层代码。

但是下一代浏览器很快就要采用WebSocket技术了，从而长轮询也会消失。在Web开发里，Node.js这种类型的技术只会变得越来越重要。

--------------------- 

## Node.js的优势
### 1、Node.js有超强的高并发能力
Node.js的首发目标，是提供一种简单的，用语创建高性能服务器及在该服务器中运行各种应用程序的开发工具。

 相对于Java，PHP或者.net 等经典服务器端语言中，Node.js正像一个年轻力胜的小伙子，Java语言会为每一个客户端创建一个新的线程，而每一个客户端连接创建一个线程，需要耗费2MB的内存。也就是说。理论上一个8GB的服务器可以同时连接用户数为4000个左右，要存在高并发支持更多的用户，必须要额外增加服务器。

Node.js不为每个客户连接创建一个新的线程，而仅仅使用一个线程。

这就是Node基于单线程（只有一个主线程去接请求，给响应）

那这不是更慢吗？事实上，并不是这样。

Node.js当接收到一个用户连接，就会触发一个内部事件。通过事先定义好的函数，达到响应用户的行为。Node.js主线程并不关心程序要走什么流程，实际上，有另外的工作线程去帮Node主线程去存取文件，读数据库，当工作线程读取到文件数据，或数据库里面的数据，就会把回调函数返回给Node主线程去执行，例如 把找到的数据传回客户端，关闭连接一些操作。（这就是Node非阻塞I/O，基于事件驱动）。



这时候我们脑袋里面应该有个雏形，就是——Node.js主线程一直在接收请求和响应请求这个活里面倒腾，这样它就可以不停地接收多个客户端发过来的请求，它不用傻傻去等待IO操作，IO工作线程找到了数据，就会触发事件回调函数告诉主线程数据已经拿到了，这时候主线就执行回调函数，把数据返回给客户端。

理论上，一个8G内存的服务器，可以同时容纳3到4万用户的连接。

这就是Node的闪光之处（单线程，非阻塞IO，事件驱动）



### 2、Node用的就是JavaScript的语法

 Node.JS 基于 javaScript 的 V8引擎，也就是说只要会JS的语法，就能用于后端开发，但是Node官方推荐用ECMA Script6（ES6）语法 。

Node打破了过去JavaScript只能在浏览器运行的局面，让前后端编程环境统一，这样就大大降低了开发成本。(这一点对前端开发人员非常友好，JS能做的东西越来越多，前端发展就越来越快)



### 3、Node.js适合做什么
![nodejs](/images/20184/nodejs1.png)

### 4、Node不适应的场景

#### ①CPU计算密集型的程序

在Node.js 0.8 版本之前，Node.js 不支持多线程。当然，这是一种设计哲学问题，因为Node.js的开发者和支持者坚信单线程和事件驱动的异步式编程比传统的多线程编程运行效率更高。但事实上多线程可以达到同样的吞吐量，尽管可能开销不小，但不必为多核环境进行特殊的配置。相比之下，Node.js 由于其单线程性的特性，必须通过多进程的方法才能充分利用多核资源。

理想情况下，Node.js单线程在执行的过程中会将一个CPU核心完全占满，所有的请求必须等待当前请求处理完毕以后进入事件循环才能响应。如果一个应用是计算密集型的，那么除非你手动将它拆散，否则请求响应延迟将会相当大。例如，某个事件的回调函数中要进行复杂的计算，占用CPU 200毫秒，那么事件循环中所有的请求都要等待200毫秒。为了提高响应速度，你唯一的办法就是把这个计算密集的部分拆成若干个逻辑，这给编程带来了额外的复杂性。即使这样，系统的总吞吐量和总响应延迟也不会降低，只是调度稍微公平了一些。不过好在真正的Web 服务器中，很少会有计算密集的部分，如果真的有，那么它不应该被实现为即时的响应。正确的方式是给用户一个提示，说服务器正在处理中，完成后会通知用户，然后交给服务器的其他进程甚至其他专职的服务器来做这件事。

#### ②单用户多任务型应用
前面我们讨论的通常都是服务器端编程，其中一个假设就是用户数量很多。但如果面对的是单用户，譬如本地的命令行工具或者图形界面，那么所谓的大量并发请求就不存在了。于是另一个恐怖的问题出现了，尽管是单用户，却不一定是单任务。例如给用户提供界面的同时后台在进行某个计算，为了让用户界面不出现阻塞状态，你不得不开启多线程或多进程。而Node.js 线程或进程之间的通信到目前为止还很不便，因为它根本没有锁，因而号称不会死锁。Node.js 的多进程往往是在执行同一任务，通过多进程利用多处理器的资源，但遇到多进程相互协作时，就显得捉襟见肘了。

#### ③逻辑十分复杂的事务
Node.js 的控制流不是线性的，它被一个个事件拆散，但人的思维却是线性的，当你试图转换思维来迎合语言或编译器时，就不得不作出牺牲。举例来说，你要实现一个这样的逻辑：从银行取钱，拿钱去购买某个虚拟商品，买完以后加入库存数据库，这中间的任何一步都可能会涉及数十次的I/O操作，而且任何一次操作失败以后都要进行回滚操作。这个过程是线性的，已经很复杂了，如果要拆分为非线性的逻辑，那么其复杂程度很可能就达到无法维护的地步了。Node.js更善于处理那些逻辑简单但访问频繁的任务，而不适合完成逻辑十分复杂的工作。







More info:
 [CSDN](https://blog.csdn.net/mozuncangtianbaxue/article/details/78393839)
 [CSDN](https://blog.csdn.net/weixin_37823121/article/details/82250852)

