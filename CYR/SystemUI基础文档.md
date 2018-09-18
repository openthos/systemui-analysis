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

    - SystemUI类图：
  - 执行流程	
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
