## StartupMenu卡的原因：
  - 当打开多个应用时，消耗更多系统内存，系统将StartupMenu的进程杀死，这时再启动 StartupMenu则是重新启动进程，造成启动慢。
 
## StartupMenu代码部分：
  - 经过陈鹏和王之旭的分析，StartupMenu的线程部分存在内存泄露的可能， 需要进一步优化。
  
## 执行计划：
  - 将StartupMenu放入SystemUI, 共用SystemUI的进程。
  - StartupMenu的进一步优化，罗俊欢来解决。


***
- 2017年6月13日

StartupMenu移植到SystemUI后,出现莫名的bug:
  - StartupMenu的GrideView添加了背景.
  - 打开StartupMenu后再次点击StartupMenu造成Systemui进程死掉(之前没有移植前就存在, 杀死StartupMenu的进程)
