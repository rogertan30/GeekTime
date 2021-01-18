## 扩展阅读
[漫谈 iOS Crash 收集框架](https://mp.weixin.qq.com/s?__biz=MjM5NTIyNTUyMQ==&mid=208483273&idx=1&sn=37ee88e06e7426f59f3074c536370317&scene=21)

[iOS 性能优化实践：头条抖音如何实现 OOM 崩溃率下降50%+](https://juejin.cn/post/6885144933997494280)

[如何判定发生了OOM](https://juejin.cn/post/6844904035787472910)

[分析字节跳动解决OOM的在线Memory Graph技术实现](https://juejin.cn/post/6895583288451465230)

## 课程笔记
[思维导图笔记](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E4%B8%B4%E8%BF%91OOM%EF%BC%8C%E5%A6%82%E4%BD%95%E8%8E%B7%E5%8F%96%E8%AF%A6%E7%BB%86%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D%E4%BF%A1%E6%81%AF%EF%BC%8C%E5%88%86%E6%9E%90%E5%86%85%E5%AD%98%E9%97%AE%E9%A2%98%EF%BC%9F/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE_withMarginNotes.pdf)

[课程原文件](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E4%B8%B4%E8%BF%91OOM%EF%BC%8C%E5%A6%82%E4%BD%95%E8%8E%B7%E5%8F%96%E8%AF%A6%E7%BB%86%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D%E4%BF%A1%E6%81%AF%EF%BC%8C%E5%88%86%E6%9E%90%E5%86%85%E5%AD%98%E9%97%AE%E9%A2%98%EF%BC%9F/14%E4%B8%A8%E4%B8%B4%E8%BF%91%20OOM%EF%BC%8C%E5%A6%82%E4%BD%95%E8%8E%B7%E5%8F%96%E8%AF%A6%E7%BB%86%E5%86%85%E5%AD%98%E5%88%86%E9%85%8D%E4%BF%A1%E6%81%AF%EF%BC%8C%E5%88%86%E6%9E%90%E5%86%85%E5%AD%98%E9%97%AE%E9%A2%98%EF%BC%9F.html)

## 摘要

使用 scalable_zone 分配内存的函数都会调用 malloc_logger 函数，因为系统总是需要有一个地方来统计并管理内存的分配情况。

其他使用 scalable_zone 分配内存的函数的方法也类似，所有大内存的分配，不管外部函数是怎么包装的，最终都会调用 malloc_logger 函数。这样的话，问题就好解决了，你可以使用 fishhook 去 Hook 这个函数，加上自己的统计记录就能够通盘掌握内存的分配情况。出现问题时，将内存分配记录的日志捞上来，你就能够跟踪到导致内存不合理增大的原因了。
