# 功能需求
  - 1.seaf-cli status获取动态显示到通知栏；
  - 2.Timer日常1min查看一次status，检测到动态后Timer周期改为就1秒，获取状态信息发送到通知栏；
  - 3.Timer日常工作的同时FileObserver监听DATA和UserConfig，只监听一层，监听到动态即停止监听，Timer周期改为1秒，发送通知;
  
## 实现
- 
