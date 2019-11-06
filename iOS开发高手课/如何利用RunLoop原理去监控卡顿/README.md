## 扩展阅读
[iOS应用UI线程卡顿监控](http://mrpeak.cn/blog/ui-detect/)

[iOS-卡顿监测-FPS监测](https://juejin.im/entry/5c8cc988e51d4552775db9d2)

## 课程笔记
[思维导图笔记](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E5%A6%82%E4%BD%95%E5%88%A9%E7%94%A8RunLoop%E5%8E%9F%E7%90%86%E5%8E%BB%E7%9B%91%E6%8E%A7%E5%8D%A1%E9%A1%BF/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE_withMarginNotes.pdf)

[课程原文件](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E5%A6%82%E4%BD%95%E5%88%A9%E7%94%A8RunLoop%E5%8E%9F%E7%90%86%E5%8E%BB%E7%9B%91%E6%8E%A7%E5%8D%A1%E9%A1%BF/13%E4%B8%A8%E5%A6%82%E4%BD%95%E5%88%A9%E7%94%A8%20RunLoop%20%E5%8E%9F%E7%90%86%E5%8E%BB%E7%9B%91%E6%8E%A7%E5%8D%A1%E9%A1%BF%EF%BC%9F.html)

## 摘要

#### 导致卡顿问题的几种原因：

* 复杂 UI 、图文混排的绘制量过大
* 在主线程上做网络同步请求
* 在主线程做大量的 IO 操作
* 运算量过大，CPU 持续高占用
* 死锁和主子线程抢锁

#### 通过监控 RunLoop 的状态来判断是否会出现卡顿

如果 RunLoop 的线程，进入睡眠前方法的执行时间过长而导致无法进入睡眠，或者线程唤醒后接收消息时间过长而无法进入下一步的话，就可以认为是线程受阻了。如果这个线程是主线程的话，表现出来的就是出现了卡顿。

所以，如果我们要利用 RunLoop 原理来监控卡顿的话，就是要关注这两个阶段。RunLoop 在进入睡眠之前和唤醒后的两个 loop 状态定义的值，分别是 kCFRunLoopBeforeSources 和 kCFRunLoopAfterWaiting ，也就是要触发 Source0 回调和接收 mach_port 消息两个状态。

#### 如何检查卡顿？
要想监听 RunLoop，你就首先需要创建一个 CFRunLoopObserverContext 观察者

将创建好的观察者 runLoopObserver 添加到主线程 RunLoop 的 common 模式下观察。然后，创建一个持续的子线程专门用来监控主线程的 RunLoop 状态。

一旦发现进入睡眠前的 kCFRunLoopBeforeSources 状态，或者唤醒后的状态 kCFRunLoopAfterWaiting，在设置的时间阈值内一直没有变化，即可判定为卡顿。接下来，我们就可以 dump 出堆栈的信息，从而进一步分析出具体是哪个方法的执行时间过长。

#### 如何获取卡顿的方法堆栈信息？
子线程监控发现卡顿后，还需要记录当前出现卡顿的方法堆栈信息，并适时推送到服务端供开发者分析，从而解决卡顿问题。那么，在这个过程中，如何获取卡顿的方法堆栈信息呢？

获取堆栈信息的一种方法是直接调用系统函数。这种方法的优点在于，性能消耗小。但是，它只能够获取简单的信息，也没有办法配合 dSYM 来获取具体是哪行代码出了问题，而且能够获取的信息类型也有限。这种方法，因为性能比较好，所以适用于观察大盘统计卡顿情况，而不是想要找到卡顿原因的场景。

另一种方法是，直接用 PLCrashReporter这个开源的第三方库来获取堆栈信息。这种方法的特点是，能够定位到问题代码的具体位置，而且性能消耗也不大。所以，也是我推荐的获取堆栈信息的方法。


