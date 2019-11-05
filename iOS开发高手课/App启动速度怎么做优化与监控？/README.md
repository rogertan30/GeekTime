
# 扩展阅读

[深入理解iOS App的启动过程](https://blog.csdn.net/Hello_Hwc/article/details/78317863)

[WWDC2016-406-Optimizing App Startup Time](https://developer.apple.com/videos/play/wwdc2016/406)

优化应用程序启动时间(可调中文字幕)

[WWDC2017-413-App Startup Time:Past,Present,and Future](https://developer.apple.com/videos/play/wwdc2017/413)

应用程序启动时间：过去、现在和未来(可调中文字幕)

[如何精准度量iOSAPP启动时间](https://www.jianshu.com/p/c14987eee107)
1. Xcode 测量 pre-main 时间的两种方法
2. 监控 C++ 静态对象的 initializer 和 ObjC Load 耗时的方法

[优化 App 的启动时间-杨萧玉](http://yulingtianxia.com/blog/2016/10/30/Optimizing-App-Startup-Time/)
1.  WWDC 2016 Session 406 的学习笔记

[iOS客户端启动速度优化-今日头条](https://techblog.toutiao.com/2017/01/17/iosspeed/#more)

* main()调用之前的耗时
    1. 减少不必要的framework，因为动态链接比较耗时
    2. check framework应当设为optional和required，如果该framework在当前App支持的所有iOS系统版本都存在，那么就设为required，否则就设为optional，因为optional会有些额外的检查
    3. 合并或者删减一些OC类，关于清理项目中没用到的类，使用工具AppCode代码检查功能
    4. 删减没有被调用到或者已经废弃的方法
    5. 将不必须在+load方法中做的事情延迟到+initialize中
    6. 尽量不要用C++虚函数(创建虚函数表有开销)
    
* main()调用之后的加载时间
    1. 不使用xib，直接视用代码加载首页视图
    2. NSUserDefaults实际上是在Library文件夹下会生产一个plist文件，如果文件太大的话一次能读取到内存中可能很耗时，这个影响需要评估，如果耗时很大的话需要拆分(需考虑老版本覆盖安装兼容问题)
    3. 每次用NSLog方式打印会隐式的创建一个Calendar，因此需要删减启动时各业务方打的log，或者仅仅针对内测版输出log
    4. 梳理应用启动时发送的所有网络请求，是否可以统一在异步线程请求

[iOS App 启动性能优化-WiFi管家](https://mp.weixin.qq.com/s/Kf3EbDIUuf0aWVT-UCEmbA)

* 应该在400ms内完成main()函数之前的加载

1. 移除不需要用到的动态库
2. 移除不需要用到的类
3. 合并功能类似的类和扩展（Category）
4. 压缩资源图片
5. 优化applicationWillFinishLaunching
6. 优化rootViewController加载

[iOS App如何优化启动时间-Facebook](http://www.cocoachina.com/ios/20160104/14870.html)

我们提出了一个创造性的解决方案 — UDP 启动。本质上，我们在通过 TCP 发送摘要请求时，先发送一个编码过的包含摘要请求的 UDP 包到服务器。这样做的目的是唤醒服务器更早地去获取和缓存数据。当真正的摘要请求通过 TCP 到达时，服务器只需见到地从缓存内容中构造出响应，并发回客户端。这个技术使得我们可以减少几百毫秒的耗时。

[一次立竿见影的启动时间优化](https://juejin.im/post/5a31190751882559e225a775)

首屏渲染优化经验

[obj中国-Mach-O 可执行文件](https://objccn.io/issue-6-3/)

[iOS app启动速度研究实践](https://zhuanlan.zhihu.com/p/38183046?from=1086193010&wm=3333_2001&weiboauthoruid=1690182120)

[iOS App冷启动治理：来自美团外卖的实践](https://mp.weixin.qq.com/s/jN3jaNrvXczZoYIRCWZs7w)

[iOS动态库的使用](https://juejin.im/post/5b1f1d3a6fb9a01e6e2baded)

[App启动之Dyld在做什么](https://juejin.im/post/5c8e278d51882545b32e657f)

# 课程笔记

[思维导图笔记](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/App%E5%90%AF%E5%8A%A8%E9%80%9F%E5%BA%A6%E6%80%8E%E4%B9%88%E5%81%9A%E4%BC%98%E5%8C%96%E4%B8%8E%E7%9B%91%E6%8E%A7%EF%BC%9F/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE_withMarginNotes.pdf)

[课程原文件](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/App%E5%90%AF%E5%8A%A8%E9%80%9F%E5%BA%A6%E6%80%8E%E4%B9%88%E5%81%9A%E4%BC%98%E5%8C%96%E4%B8%8E%E7%9B%91%E6%8E%A7%EF%BC%9F/02%E4%B8%A8App%20%E5%90%AF%E5%8A%A8%E9%80%9F%E5%BA%A6%E6%80%8E%E4%B9%88%E5%81%9A%E4%BC%98%E5%8C%96%E4%B8%8E%E7%9B%91%E6%8E%A7%EF%BC%9F.html)

[小码哥底层班-启动优化](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/App%E5%90%AF%E5%8A%A8%E9%80%9F%E5%BA%A6%E6%80%8E%E4%B9%88%E5%81%9A%E4%BC%98%E5%8C%96%E4%B8%8E%E7%9B%91%E6%8E%A7%EF%BC%9F/009-%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%20%E5%90%AF%E5%8A%A8%E6%97%B6%E9%97%B4.pdf)

# 摘要

一般情况下，App 的启动分为冷启动和热启动。

* 冷启动是指， App 点击启动前，它的进程不在系统里，需要系统新创建一个进程分配给它启动的情况。这是一次完整的启动过程。
* 热启动是指 ，App 在冷启动后用户将 App 退后台，在 App 的进程还在系统里的情况下，用户重新启动进入 App 的过程，这个过程做的事情非常少。

#### **究竟如何才能把启动时的所有耗时都找出来呢？解决这个问题，你首先需要弄清楚 App 在启动时都干了哪些事儿。**

一般而言，App 的启动时间，指的是从用户点击 App 开始，到用户看到第一个界面之间的时间。总结来说，App 的启动主要包括三个阶段：

1. main() 函数执行前；
2. main() 函数执行后；
3. 首屏渲染完成后。

#### main() 函数执行前
在 main() 函数执行前，系统主要会做下面几件事情：

* 加载可执行文件（App 的.o 文件的集合）；
* 加载动态链接库，进行 rebase 指针调整和 bind 符号绑定；
* Objc 运行时的初始处理，包括 Objc 相关类的注册、category 注册、selector 唯一性检查等；
* 初始化，包括了执行 +load() 方法、attribute((constructor)) 修饰的函数的调用、创建 C++ 静态全局变量。

相应地，这个阶段对于启动速度优化来说，可以做的事情包括：

* 减少动态库加载。每个库本身都有依赖关系，苹果公司建议使用更少的动态库，并且建议在使用动态库的数量较多时，尽量将多个动态库进行合并。数量上，苹果公司最多可以支持 6 个非系统动态库合并为一个。
* 减少加载启动后不会去使用的类或者方法。
* +load() 方法里的内容可以放到首屏渲染完成后再执行，或使用 +initialize() 方法替换掉。因为，在一个 +load() 方法里，进行运行时方法替换操作会带来 4 毫秒的消耗。不要小看这 4 毫秒，积少成多，执行 +load() 方法对启动速度的影响会越来越大。
* 控制 C++ 全局变量的数量。

#### main() 函数执行后

main() 函数执行后的阶段，指的是从 main() 函数执行开始，到 appDelegate 的 didFinishLaunchingWithOptions 方法里首屏渲染相关方法执行完成。

首页的业务代码都是要在这个阶段，也就是首屏渲染前执行的，主要包括了：

* 首屏初始化所需配置文件的读写操作；
* 首屏列表大数据的读取；
* 首屏渲染的大量计算等。

很多时候，开发者会把各种初始化工作都放到这个阶段执行，导致渲染完成滞后。更加优化的开发方式，应该是从功能上梳理出哪些是首屏渲染必要的初始化功能，哪些是 App 启动必要的初始化功能，而哪些是只需要在对应功能开始使用时才需要初始化的。梳理完之后，将这些初始化功能分别放到合适的阶段进行。

#### 首屏渲染完成后

首屏渲染后的这个阶段，主要完成的是，非首屏其他业务服务模块的初始化、监听的注册、配置文件的读取等。从函数上来看，这个阶段指的就是截止到 didFinishLaunchingWithOptions 方法作用域内执行首屏渲染之后的所有方法执行完成。简单说的话，这个阶段就是从渲染完成时开始，到 didFinishLaunchingWithOptions 方法作用域结束时结束。

这个阶段用户已经能够看到 App 的首页信息了，所以优化的优先级排在最后。但是，那些会卡住主线程的方法还是需要最优先处理的，不然还是会影响到用户后面的交互操作。

明白了 App 启动阶段需要完成的工作后，我们就可以有的放矢地进行启动速度的优化了。这些优化，包括了功能级别和方法级别的启动优化。接下来，我们就从这两个角度展开看看。

#### 功能级别的启动优化

优化的思路是： main() 函数开始执行后到首屏渲染完成前只处理首屏相关的业务，其他非首屏业务的初始化、监听注册、配置文件读取等都放到首屏渲染完成后去做。如下图所示：

#### 方法级别的启动优化

在这之后，我们需要进一步做的，是检查首屏渲染完成前主线程上有哪些耗时方法，将没必要的耗时方法滞后或者异步执行。通常情况下，耗时较长的方法主要发生在计算大量数据的情况下，具体的表现就是加载、编辑、存储图片和文件等资源。

**第一种方法是，定时抓取主线程上的方法调用堆栈，计算一段时间里各个方法的耗时**。Xcode 工具套件里自带的 Time Profiler ，采用的就是这种方式。

**第二种方法是，对 objc_msgSend 方法进行 hook 来掌握所有方法的执行耗时**。

hook 方法的意思是，在原方法开始执行时换成执行其他你指定的方法，或者在原有方法执行前后执行你指定的方法，来达到掌握和改变指定方法的目的。

hook objc_msgSend 这种方式的优点是非常精确，而缺点是只能针对 Objective-C 的方法。当然，对于 c 方法和 block 也不是没有办法，你可以使用 libffi 的 ffi_call 来达成 hook，但缺点就是编写维护相关工具门槛高。

#### 如何做一个方法级别启动耗时检查工具来辅助分析和监控？

[耗时监控](https://github.com/ming1016/RSSRead)
