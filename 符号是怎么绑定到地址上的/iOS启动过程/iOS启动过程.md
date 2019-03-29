1. 加载dyld到App进程 
    * dyld会首先读取mach-o文件的Header和load commands。 
接着就知道了这个可执行文件依赖的动态库。例如加载动态库A到内存，接着检查A所依赖的动态库，就这样的递归加载，直到所有的动态库加载完毕。通常一个App所依赖的动态库在100-400个左右，其中大多数都是系统的动态库，它们会被缓存到dyld shared cache，这样读取的效率会很高。

2. 加载动态库（包括所依赖的所有动态库） 

3. Rebase 
    * Rebase 修正内部(指向当前mach-o文件)的指针指向

4. Bind 
   * Bind 修正外部指针指向 
5. 初始化Objective C Runtime 
   * Objective C是动态语言，所以在执行main函数之前，需要把类的信息注册到一个全局的Table中。同时，Objective C支持Category，在初始化的时候，也会把Category中的方法注册到对应的类中，同时会唯一Selector，这也是为什么当你的Cagegory实现了类中同名的方法后，类中的方法会被覆盖。

6. 其它的初始化代码
    * +load方法。
    * C／C++静态初始化对象和标记为__attribute__(constructor)的方法