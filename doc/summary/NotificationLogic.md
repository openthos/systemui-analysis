## 通知栏的代码分析思路：
  - 1.从布局入手：status_bar_expanded.xml
    - NotificationPanelView.java 整体通知栏的自定义View
    - 通知消息：LinearLayout 线性布局中嵌套 帧布局 ， 消息：NotificationStackScrollLayout.java
    - 打印部分 ， 这部分布局就比较清除。
    - 快速设置部分： qs_panel.xml值得注意。
