* Mach-O是Mach object的缩写，是Mac/iOS上用于存储程序，库的标准格式
* 属于Mach-O格式的文件类型有
![屏幕快照 2019-03-27 下午10.48.15.png](https://github.com/rogertan30/GeekTime/blob/master/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/Mach-O/resources/2A79DEA87CEBF56EC519F450C39113BC.png)

## 常见的Mach-O文件类型
* MH_OBJECT
  * 目标文件（.o），c语言打包成可执行文件过程中的中间产物。即1.c 2.c -> 1.o 2.0 -> 链接 -> 可执行文件
  * 静态库文件（.a），静态库其实就是N个.o文件合并在一起
* MH_EXECUTE 
  * 可执行文件
* MH_DYLIB
  * 动态库文件
  * .dylib
  * .framework
* MH_DYLINKER
  * 动态链接编辑器
  * /usr/lib/dyld
* MH_DSYM
  * 存储着二进制文件符号信息的文件
  * 常用于分析app的崩溃信息
  
## 通用二进制文件 Universal Binary
![屏幕快照 2019-03-28 上午12.07.49.png](https://github.com/rogertan30/GeekTime/blob/master/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/Mach-O/resources/00864DA3EB12086AFE7C571F08B322F4.png =486x207)

## Mach-O的结构
![屏幕快照 2019-03-28 上午12.09.03.png](https://github.com/rogertan30/GeekTime/blob/master/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/Mach-O/resources/CA562A91EE2C5D9F0FE681F30CFAA314.png =407x437)

### dyld和Mach-O
* dyld用于加载一下集中Mach-O
  * MH_EXECUTE
  * MH_DYLIB
  * MH_BUNDLE
* APP的可执行文件，动态库都是由dyld负责加载的

![屏幕快照 2019-03-28 上午12.49.03.png](https://github.com/rogertan30/GeekTime/blob/master/%E7%AC%A6%E5%8F%B7%E6%98%AF%E6%80%8E%E4%B9%88%E7%BB%91%E5%AE%9A%E5%88%B0%E5%9C%B0%E5%9D%80%E4%B8%8A%E7%9A%84/Mach-O/resources/80C2AB3F34B8B96ABD0B8E09A3D63F6F.png =666x373)
