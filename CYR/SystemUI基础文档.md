## SystemUI基础(asop)
***
##### 该文档和其他文档有重复，属于整理的全部基础，便于宏观掌握
## 目录
  - 需求
    - 整体功能结构图	
      - 1. 通知栏
        - 1. wifi 飞行模式　手电筒　设置　..
      - 2. 状态栏
        - 时间　日历　信号 ..
    - 部分界面，功能描述
  - 代码结构
    - 源码结构和资源文件
      - com.android.systemui.statusbar
      - com.android.systemui.statusbar.phone
      - com.android.systemui.statusbar.policy
      - com.android.systemui.statusbar.stack
      - com.android.systemui.statusbar.tv
    - 通知栏，关键类和资源文件
      - SystemUI/src/com/android/systemui/statusbar/phone/PhoneStatusBar.java
      - base/packages/SystemUI/src/com/android/systemui/SystemUIService.java
      - frameworks/base/services/java/com/android/server/SystemServer.java

      - packages/SystemUI/src/com/android/systemui/statusbar/SystemBars.java
      - packages/SystemUI/src/com/android/systemui/statusbar/BaseStatusBar.java
      - packages/SystemUI/src/com/android/systemui/SystemUIApplication.java

      - base/packages/SystemUI/src/com/android/systemui/qs/QSPanel.java
      - src/com/android/systemui/statusbar/policy/BrightnessMirrorController.java
      - packages/SystemUI/src/com/android/systemui/recent/RecentsPanelView.java
      - SystemUI/src/com/android/systemui/statusbar/phone/NavigationBarView.java
      - packages/SystemUI/src/com/android/systemui/volume/VolumePanel.java

      - frameworks/base/packages/SystemUI/res/layout/super_status_bar.xml
      - frameworks/base/packages/SystemUI/res/layout/navigation_bar.xml
      - frameworks/base/packages/SystemUI/res/layout/system_icons.xml

      - frameworks/base/packages/SystemUI/res/layout/status_bar_expanded.xml
      - frameworks/base/packages/SystemUI/res/layout/status_bar_expanded_header.xml
      - frameworks/base/packages/SystemUI/res/layout/qs_panel.xml

    - SystemUI类图
      - ![](https://github.com/openthos/systemui-analysis/blob/master/CYR/icon/systemui.png)
      - 状态的核心类是BaseStatusBar，这个类是一个抽象类。它的start()方法(继承自SystemUI)定义了状态栏启动时的具体步骤。
      - BaseStatusBar继承自SystemUI，SystemUI被SystemUIService调用，SystemUIService继承Service，所以StatuBar也是一个Service。
      - BaseStatusBar实现了CommandQueue.Callbacks接口，同时可以发现CommandQueue继承自IStatusBar.Stub远程接口，而IStatusBar.Stub接口的方法则通过CommandQueue的Callbacks接口实现，所以说BaseStatusBar又是IStatusBar.stub远程接口的实现类。　　
      - 我们说BaseStatusBar是抽象类，那么IStatusBar.stub接口中方法的实现该如何实现呢？很简单，我们可以通过StatuBar的两个子类：PhoneStatusBar和TabletStatusBar来实现。
      - 另外KeyguardViewMediator, RingtonePlayer, VolumeUI, SystemBars, PowerUI, StorageNotification, Recents 这几个UI组件也继承自 SystemUI
    - 执行流程
      - SystemUI是为用户提供系统级别的信息显示与交互的一套UI组件，因此它所实现的功能包罗万象:
      - 屏幕顶端的状态栏、
      - 底部的导航栏、
      - 图片壁纸以及RecentPanel（近期使用的APP列表）都属于SystemUI的范畴。
      - SystemUI中还有一个名为TakeScreenshotService的服务，用于在用户按下音量下键与电源键时进行截屏操作。
      - SystemUI还提供了PowerUI和RingtonePlayer两个服务。前者负责监控系统的剩余电量并在必要时为用户显示低电警告，后者则依托AudioService为向其他应用程序提供播放铃声的功能。
      - SystemUI大部分功能之间相互独立:
      - 比如RecentPanel、TakeScreenshotService等均是按需启动，并在完成其既定任务后退出，这与普通的Activity以及Service别无二致。
      - 比较特殊的是状态栏、导航栏等组件的启动方式。它们运行于一个称之为SystemUIService的一个Service之中。因此讨论状态栏与导航栏的启动过程其实就是SystemUIService的启动过程。
    - SystemUI启动	
    - NavigationBar导航栏	
  - RecentsActivity最近的APP	
    - 第三方APP访问Recent	
  - StatusBar加图标AddIcons	
    - Icons排列规则	
    - QuickSettingPanel快捷开关	
  - ScreenShot事件流程	
  - APP与SystemUI交互	
    - APP通知到PhoneStatusBar	
    - APP清除(Cancel)通知	
