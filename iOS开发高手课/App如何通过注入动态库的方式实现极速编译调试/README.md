> iOS 原生代码的编译调试，都是通过一遍又一遍地编译重启 App 来进行的

## Injection for Xcode

Injection 会监听源代码文件的变化，如果文件被改动了，Injection Server 就会执行 rebuildClass 重新进行编译、打包成动态库，也就是 .dylib 文件。编译、打包成动态库后使用 writeSting 方法通过 Socket 通知运行的 App。

Client 接收到消息后会调用 inject(tmpfile: String) 方法，运行时进行类的动态替换

inject(tmpfile: String) 方法的代码大部分都是做新类动态替换旧类。

inject(tmpfile: String) 的入参 tmpfile 是动态库的文件路径

当类的方法都被替换后，我们就可以开始重新绘制界面了。整个过程无需重新编译和重启 App，至此使用动态库方式极速调试的目的就达成了。

![性能问题](https://github.com/rogertan30/GeekTime/tree/master/App如何通过注入动态库的方式实现极速编译调试/images/原理.png)

