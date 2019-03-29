## 传统编译器架构

![屏幕快照 2019-03-26 下午1.54.06.png](resources/716C2AEBEAF9B409A7298AD17F8DF402.png =471x88)

* Frontend：前端
  * 词法分析，语法分析，语义分析，生成中间代码Optimizer
* Optimizer：优化器
  * 中间代码优化，更快，更小
* Backend：后端
  * 生成机器码
  * 如果运行在iOS上，则生成ARM架构的代码，如果运行在Win上，则生成x86，x64架构的代码
  
## LLVM编译器架构

![屏幕快照 2019-03-26 下午7.30.01.png](resources/2AC91DAEF6DFDB9AFB0FFFE95B3A4C48.png =565x213)
* 同样分为前端，优化器，后端
* 后段负责生成不同平台，不同架构的机器码
* 不同编程语言对应的前端不一样
* 不同的前端后端使用统一的中间代码LLVM IR
* 如果需要支持一种新的编程语言，那么只需要实现一个新的前端
* 如果需要支持一种新的硬件设备，那么只需要实现一个新的后端
* 优化阶段是一个通用的阶段，它针对的是统一的LLVM IR，不论是支持新的编程语言，还是支持新的硬件设备，都不需要对优化阶段进行改变。
* llvm现在被作为实现各种静态和运行时编译语言的通用基础结构。（java，python，Ruby）

## Clang

* 什么是Clang
  * LLVM项目的一个子项目
  * 基于LLVM架构的c/c++/OC编译器前端

* 相比较GCC，Clang具有哪些优点
  * 编译速度快：在某些平台上，Clang的编译速度显著的快过GCC
  * 占用内存小：Clang生成的AST（语法树）所占用的内存是GCC的五分之一左右
  * 模块化设计：Clang采用基于库的模块化设计，易于IDE集成及其他用途的重用
  * 诊断信息可读性强：在编译过程中，Clang创建并保存了大量详细的元数据，有利于调试
  * 设计清晰简单，容易理解，易于扩展增强
  
## Clang与LLVM

![屏幕快照 2019-03-27 上午10.39.52.png](resources/F5D8B397FD886C49325FB882E0937EF8.png =474x328)

* 广义的LLVM
  * 整个LLVM架构
  
* 狭义的LLVM
  * LLVM后端（代码优化，目标代码生成）
  
  
![屏幕快照 2019-03-27 上午10.41.55.png](resources/4694E80E7971274E660D7A1456DFAC4F.png =610x132)

* 左边是编程语言，最右边是机器码
* 首先通过编译器前端Clang，生成中间代码IR
* 中间代码要经过一系列的优化，优化用的是Pass
* 中间代码的优化可以自己编写，可以插入一个Pass

## OC源文件的编译过程
![屏幕快照 2019-03-27 上午10.56.58.png](resources/AF3DA1C6FFB601FD733C74E327BEE3FD.png =487x208)
0. 找到main.m文件
1. 预处理器，把include，import，宏定义替换掉
    * 把include中的文件拷贝到当前.m文件中
    * 查看preprocessor(预处理)的结果： $ clang -E main.m
2. 编译器编译为中间代码IR
    * 词法分析，生成Token：  $ clang -fmodules -E -Xclang -dump-tokens main.m
      * ![屏幕快照 2019-03-27 上午11.07.48.png](resources/09E702E0F6BC4C4608DFB844B764739D.png =487x346)
    * 语法分析，生成语法树（AST，Abstract Syntax Tree）：$ clang -fmodules -fsyntax-only -Xclang -ast-dump main.m
      * ![屏幕快照 2019-03-27 上午11.23.00.png](resources/A7355D0C6473881A9853D5656E4450D9.png =208x73) 
      * ![屏幕快照 2019-03-27 上午11.22.23.png](resources/4375B09E7C5AB42D7E1BCD6E5AE07769.png =624x196)3. 交给后端生成目标代码
4. 目标代码
5. 链接动态库，静态库
6. 变成适合某个架构的代码

## 语法树 AST
![屏幕快照 2019-03-27 下午9.27.01.png](resources/CF980BB3E5F714BDDF3F6A4EF7B32850.png =651x423)

## 中间代码 LLVM IR
* IR有3种表示形式（但本质是等价的，就好比水可以有气体，液体，固体3种形式）
  * text：便于阅读的文本格式，类似于汇编语言，拓展名.II $ clang -S -emit-llvm main.m
  * memory：内存格式
  * bitcode：二进制格式，拓展名.bc $ clang -c -emit-llvm main.m
![屏幕快照 2019-03-27 下午9.33.58.png](resources/9C2DCD6D19D0FC004E91623498191F6F.png =450x232)

## 安装Clang

![屏幕快照 2019-03-27 下午9.38.05.png](resources/27C9A54C7B527EFA257B648BE17316C8.png =292x233)
![屏幕快照 2019-03-27 下午9.40.31.png](resources/CF2A595BB57148BDF21985BA5B6F0E83.png =578x390)
![屏幕快照 2019-03-27 下午9.50.40.png](resources/51CF15E327BF564D2F1A561546BA2F43.png =514x347)

## Clang 插件开发