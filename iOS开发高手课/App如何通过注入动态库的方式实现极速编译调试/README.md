## 扩展阅读
[Injection：iOS热重载背后的黑魔法](https://juejin.im/entry/5b1f4c5f5188257d7c35e9d9)

## 课程笔记
[思维导图笔记](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/App%E5%A6%82%E4%BD%95%E9%80%9A%E8%BF%87%E6%B3%A8%E5%85%A5%E5%8A%A8%E6%80%81%E5%BA%93%E7%9A%84%E6%96%B9%E5%BC%8F%E5%AE%9E%E7%8E%B0%E6%9E%81%E9%80%9F%E7%BC%96%E8%AF%91%E8%B0%83%E8%AF%95/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE_withMarginNotes.pdf)

[课程原文件](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/App%E5%A6%82%E4%BD%95%E9%80%9A%E8%BF%87%E6%B3%A8%E5%85%A5%E5%8A%A8%E6%80%81%E5%BA%93%E7%9A%84%E6%96%B9%E5%BC%8F%E5%AE%9E%E7%8E%B0%E6%9E%81%E9%80%9F%E7%BC%96%E8%AF%91%E8%B0%83%E8%AF%95/06%E4%B8%A8App%20%E5%A6%82%E4%BD%95%E9%80%9A%E8%BF%87%E6%B3%A8%E5%85%A5%E5%8A%A8%E6%80%81%E5%BA%93%E7%9A%84%E6%96%B9%E5%BC%8F%E5%AE%9E%E7%8E%B0%E6%9E%81%E9%80%9F%E7%BC%96%E8%AF%91%E8%B0%83%E8%AF%95%EF%BC%9F.html)

## 摘要

> iOS 原生代码的编译调试，都是通过一遍又一遍地编译重启 App 来进行的

#### Injection for Xcode

Injection 会监听源代码文件的变化，如果文件被改动了，Injection Server 就会执行 rebuildClass 重新进行编译、打包成动态库，也就是 .dylib 文件。编译、打包成动态库后使用 writeSting 方法通过 Socket 通知运行的 App。

Client 接收到消息后会调用 inject(tmpfile: String) 方法，运行时进行类的动态替换

inject(tmpfile: String) 方法的代码大部分都是做新类动态替换旧类。

inject(tmpfile: String) 的入参 tmpfile 是动态库的文件路径

当类的方法都被替换后，我们就可以开始重新绘制界面了。整个过程无需重新编译和重启 App，至此使用动态库方式极速调试的目的就达成了。


#### 整个过程总结

1. 创建监听SimpleSocket，通过File Watcher监听观察文件改动
2. 修改代码，保存后重新编译修改的类文件，修改后的文件被编译为了.dylib动态库
3. 然后通过writestring给我们的App发"INJECT"消息，通知App更新代码
4. 通过SwiftEval.instance.loadAndInject方法dlopen加载.dylib动态库
5. 然后通过OC runtime 的class_replaceMethod把整个类的实现方法都替换
6. 然后再调SwiftInjected.injected我们的类收到消息开始重绘UI

