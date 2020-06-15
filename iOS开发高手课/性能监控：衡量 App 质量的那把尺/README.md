## 扩展阅读
无

## 课程笔记
[思维导图笔记](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E6%80%A7%E8%83%BD%E7%9B%91%E6%8E%A7%EF%BC%9A%E8%A1%A1%E9%87%8F%20App%20%E8%B4%A8%E9%87%8F%E7%9A%84%E9%82%A3%E6%8A%8A%E5%B0%BA/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE_withMarginNotes.pdf)

[课程原文件](https://github.com/rogertan30/GeekTime/blob/master/iOS%E5%BC%80%E5%8F%91%E9%AB%98%E6%89%8B%E8%AF%BE/%E6%80%A7%E8%83%BD%E7%9B%91%E6%8E%A7%EF%BC%9A%E8%A1%A1%E9%87%8F%20App%20%E8%B4%A8%E9%87%8F%E7%9A%84%E9%82%A3%E6%8A%8A%E5%B0%BA/16%E4%B8%A8%E6%80%A7%E8%83%BD%E7%9B%91%E6%8E%A7%EF%BC%9A%E8%A1%A1%E9%87%8F%20App%20%E8%B4%A8%E9%87%8F%E7%9A%84%E9%82%A3%E6%8A%8A%E5%B0%BA.html)

## 课程摘要

CADisplayLink:与屏幕刷新频率的计时器同步，每次屏幕刷新都会调用一次，所以可以获取到一秒钟屏幕刷新的次数。

线下监控：Instrument，一个工具检测所有。

线上监控：CPU使用直接获取所有线程的cpu_usage计算综合，内存消耗使用task_basic_info的phys_footprint，FPS用CADisplayLink。
