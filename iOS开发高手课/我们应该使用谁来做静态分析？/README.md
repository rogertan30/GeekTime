## 扩展阅读
[swift编码规范](https://github.com/Artwalk/swift-style-guide/blob/master/README_CN.md)

[SwiftLint的集成和使用](https://www.jianshu.com/p/d8fef88b26de)

[SwiftLint，规范代码，成为完美的偏执患者](https://www.jianshu.com/p/40aa8695503f)


## 课程笔记
[思维导图笔记](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E6%88%91%E4%BB%AC%E5%BA%94%E8%AF%A5%E4%BD%BF%E7%94%A8%E8%B0%81%E6%9D%A5%E5%81%9A%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90%EF%BC%9F/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE_withMarginNotes.pdf)

[课程原文件](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E6%88%91%E4%BB%AC%E5%BA%94%E8%AF%A5%E4%BD%BF%E7%94%A8%E8%B0%81%E6%9D%A5%E5%81%9A%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90%EF%BC%9F/07%E4%B8%A8Clang%E3%80%81Infer%20%E5%92%8C%20OCLint%20%EF%BC%8C%E6%88%91%E4%BB%AC%E5%BA%94%E8%AF%A5%E4%BD%BF%E7%94%A8%E8%B0%81%E6%9D%A5%E5%81%9A%E9%9D%99%E6%80%81%E5%88%86%E6%9E%90%EF%BC%9F.html)

## 摘要

> 静态分析，这种可以帮助我们在编写代码的阶段就能及时发现代码错误，从而在根儿上保证工程质量的技术，就成为了 iOS 开发者最常用到的一种代码调试技术。

#### OCLint

覆盖了具有通用性的规则，主要包括语法上的基础规则、Cocoa 库相关规则、一些约定俗成的规则、各种空语句检查、是否按新语法改写的检查、命名上长变量名短变量名检查、无用的语句变量和参数的检查。

除此之外，还包括了和代码量大小是否合理相关的一些规则，比如过大的类、类里方法是否太多、参数是否过多、Block 嵌套是否太深、方法里代码是否过多、圈复杂度的检查等。

#### Clang 静态分析器

Clang 静态分析器（Clang Static Analyzer）是一个用 C++ 开发的，用来分析 C、C++ 和 Objective-C 的开源工具，是 Clang 项目的一部分，构建在 Clang 和 LLVM 之上。Clang 静态分析器的分析引擎用的就是 Clang 的库。

#### Infer

Infer 是 Facebook 开源的、使用 OCaml 语言编写的静态分析工具，可以对 C、Java 和 Objective-C 代码进行静态分析，可以检查出空指针访问、资源泄露以及内存泄露。

#### 总结

* Clang 静态分析器和 Xcode 的集成度高，也支持命令行。不过，它们检查的规则少，基本都是只能检查出较大的问题，比如类型转换问题，而对内存泄露问题检查的侧重点则在于可用性。

* OCLint 检查规则多、定制性强，能够发现很多潜在问题。但缺点也是检查规则太多，反而容易找不到重点；可定制度过高，导致易用性变差。

* Infer 的效率高，支持增量分析，可小范围分析。可定制性不算最强，属于中等。

综合来看，Infer 在准确性、性能效率、规则、扩展性、易用性整体度上的把握是做得最好的，我认为这些是决定静态分析器好不好最重要的几点。所以，我比较推荐的是使用 Infer 来进行代码静态分析。


