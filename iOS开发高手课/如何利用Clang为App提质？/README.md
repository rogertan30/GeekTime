## 扩展阅读

## 课程笔记
[思维导图笔记](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E5%A6%82%E4%BD%95%E5%88%A9%E7%94%A8Clang%E4%B8%BAApp%E6%8F%90%E8%B4%A8%EF%BC%9F/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE_withMarginNotes.pdf)

[课程原文件](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E5%A6%82%E4%BD%95%E5%88%A9%E7%94%A8Clang%E4%B8%BAApp%E6%8F%90%E8%B4%A8%EF%BC%9F/08%E4%B8%A8%E5%A6%82%E4%BD%95%E5%88%A9%E7%94%A8%20Clang%20%E4%B8%BA%20App%20%E6%8F%90%E8%B4%A8%EF%BC%9F.html)

## 摘要
> 通过 Clang 提供的丰富接口功能就可以开发出静态分析工具，进而管控代码质量。基于 Clang 还可以开发出用于代码增量分析、代码可视化、代码质量报告来保障 App 质量的系统平台，比如CodeChecker。

#### 什么是 Clang？

Clang 是 C、C++、Objective-C 的编译前端，而 Swift 有自己的编译前端（也就是 Swift 前端多出的 SIL optimizer）

* 第一，对于使用者来说，Clang 编译的速度非常快，对内存的使用率非常低，并且兼容 GCC。

* 第二，对于代码诊断来说， Clang 也非常强大，Xcode 也是用的 Clang。使用 Clang 编译前端，可以精确地显示出问题所在的行和具体位置，并且可以确切地说明出现这个问题的原因，并指出错误的类型是什么，使得我们可以快速掌握问题的细节。这样的话，我们不用看源码，仅通过 Clang 突出标注的问题范围也能够了解到问题的情况。

* 第三，Clang 对 typedef 的保留和展开也处理得非常好。typedef 可以缩写很长的类型，保留 typedef 对于粗粒度诊断分析很有帮助。但有时候，我们还需要了解细节，对 typedef 进行展开即可。

* 第四，Fix-it 提示也是 Clang 提供的一种快捷修复源码问题的方式。在宏的处理上，很多宏都是深度嵌套的， Clang 会自动打印实例化信息和嵌套范围信息来帮助你进行宏的诊断和分析。

* 第五，Clang 的架构是模块化的。除了代码静态分析外，利用其输出的接口还可以开发用于代码转义、代码生成、代码重构的工具，方便与 IDE 进行集成。

#### Clang 做了哪些事？

* 首先，Clang 会对代码进行词法分析，将代码切分成Token。输入一个命令可以查看上面代码的所有的 Token。

* 接下来，词法分析完后就会进行语法分析，将输出的 Token 先按照语法组合成语义，生成类似 VarDecl 这样的节点，然后将这些节点按照层级关系构成抽象语法树（AST）。

#### Clang 提供了什么能力？

Clang 为一些需要分析代码语法、语义信息的工具提供了基础设施。这些基础设施就是 LibClang、Clang Plugin 和 LibTooling。

#### LibClang

LibClang 提供了一个稳定的高级 C 接口，Xcode 使用的就是 LibClang。LibClang 可以访问 Clang 的上层高级抽象的能力，比如获取所有 Token、遍历语法树、代码补全等。由于 API 很稳定，Clang 版本更新对其影响不大。但是，LibClang 并不能完全访问到 Clang AST 信息。

#### Clang Plugins

Clang Plugins 可以让你在 AST 上做些操作，这些操作能够集成到编译中，成为编译的一部分。插件是在运行时由编译器加载的动态库，方便集成到构建系统中。

#### LibTooling

LibTooling 是一个 C++ 接口，通过 LibTooling 能够编写独立运行的语法检查和代码重构工具。

* 改变代码：可以改变 Clang 生成代码的方式。基于现有代码可以做出大量的修改。还可以进行语言的转换，比如把 OC 语言转成 JavaScript 或者 Swift。

* 做检查：检查命名规范，增加更强的类型检查，还可以按照自己的定义进行代码的检查分析。

* 做分析：对源码做任意类型分析，甚至重写程序。给 Clang 添加一些自定义的分析，创建自己的重构器，还可以基于工程生成相关图形或文档进行分析。

#### 小结
在今天这篇文章中，我和你说了 Clang 做了什么，以及提供了什么能力。从中可以看出，Clang 提供的能力都是基于 Clang AST 接口的。

这个接口的功能非常强大，除了能够获取符号在源码中的位置，还可以获取方法的调用关系，类型定义和源码里的所有内容。

以这个接口为基础，再利用 LibClang、 Clang Plugin 和 LibTooling 这些封装好的工具，就足够我们去开发出满足静态代码分析需求的工具了。比如，我们可以使用 Clang Plugin 自动在构建阶段检查是否满足代码规范，不满足则直接无法构建成功。再比如，我们可以使用 LibTooling 自动完成代码的重构，与手动重构相比会更加高效、精确。

还记得我们在上一篇文章“Clang、Infer 和 OCLint ，我们应该使用谁来做静态分析？”中，提到的 Clang 静态分析器的引擎吗？它使用的就是 Clang AST 接口，对于节点 Stmt、Decl、Type 及其派生节点 Clang AST 都有对应的接口，特别是 RecursiveASTVisitor 接口可以完整遍历整个 AST。通过对 AST 的完整遍历以及节点数据获取，就能够对数据流进行分析，比如 Iterative Data Flow Analysis、path-sensitive、path-insensitive、flow-sensitive 等。

此外，还能够模拟内存分配进行分析，Clang 静态分析器里对应的模块是 MemRegion，其中内存模型是基于 “A Memory Model for Static Analysis of C Programs”这篇论文而来。在 Clang 里的具体实现代码，你可以查看 MemRegion.h 和 RegionStore.cpp 这两个文件。对于 Clang 静态分析器的原理描述，你可以参看官方说明。



