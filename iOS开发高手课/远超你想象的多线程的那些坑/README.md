## 扩展阅读
暂无

## 课程笔记
[思维导图笔记](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E8%BF%9C%E8%B6%85%E4%BD%A0%E6%83%B3%E8%B1%A1%E7%9A%84%E5%A4%9A%E7%BA%BF%E7%A8%8B%E7%9A%84%E9%82%A3%E4%BA%9B%E5%9D%91/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE_withMarginNotes.pdf)

[课程原文件](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E8%BF%9C%E8%B6%85%E4%BD%A0%E6%83%B3%E8%B1%A1%E7%9A%84%E5%A4%9A%E7%BA%BF%E7%A8%8B%E7%9A%84%E9%82%A3%E4%BA%9B%E5%9D%91/17%E4%B8%A8%E8%BF%9C%E8%B6%85%E4%BD%A0%E6%83%B3%E8%B1%A1%E7%9A%84%E5%A4%9A%E7%BA%BF%E7%A8%8B%E7%9A%84%E9%82%A3%E4%BA%9B%E5%9D%91.html)

## 文章摘要

### tripleCC评论：
关于NSURLConnection和URLSession那块有一个地方讲的感觉不是很清楚。NSURLConnection也可以设置执行代理的queue，通过setDelegateQueue：方法，本质上是NSURLConnection的机制需要创建此对象的线程的RunLoop去监听网络事件，然后执行注册的回调，所以在网络回来前，需要保持RunLoop的运行状态。NSURLSession应该是内部做了这个事情，所以发请求不需要所在线程“常驻”，而不是因为我们可以设置delegateQueue。从代理方法的调用栈看，NSURLSession的代理任务都是在com.apple.NSURLSession-work这个queue被塞进delegateQueue的，如果不设置delegateQueue的话，代理任务就在work队列执行，设置之后再派发到对应的队列。具体在work队列之前做了什么，还没看到比较具体的资料，不过基于底层都是CFNetworking，这个又是Source0，可能做的和常驻线程相似。

### HLHD评论：
通过上面的分析我们可以知道，如果不是因为 NSURLConnection 的请求必须要有一个一直存活的线程来接收回调，那么 AFNetworking 2.0 就不用创建一个常驻线程出来了。虽然说，在一个 App 里网络请求这个动作的占比很高，但也有很多不需要网络的场景，所以线程一直常驻在内存中，也是不合理的。

意思就是线程必须得活着才能收到回调方法。跟设置delegatequeue应该没关系。 

urlsession应该是内部的work线程一直是活着的。所以不需要像urlconnection那样单独开一个线程保活。
