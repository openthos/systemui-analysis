### StartupMenu的实现逻辑

***

#### 8.0 StartupMenu设计
  - 1. 在原有5.1的基础上进行的修改.
  - 2. 借用TaskBar的启动方式．
  - 3. 将StartupMenu以SystemUI的Dialog形式启动．
  - 4. 将StartupMenu移植到SystemUI中．

#### 8.0 StartupMenu结构
  - frameworks/base/packages/SystemUI/src/com/android/systemui/dialog/
       - StartupMenuDialog.java    //StartupMenu主体类
         - 添加: gride / list的视图
         - gride //右侧列表　list //左侧常用列表．
       - BaseDialog.java    //SystemUI所有Dialog的基类
  - frameworks/base/packages/SystemUI/src/com/android/systemui/StartupMenu
       - AppEntry.java //app模型类
       - DialogType.java //Dialog的几种类型
       - ShowType.java　//enum对象，区别list/grid视图　
       - SortSelectPopupWindow.java　//time/click/name/default/排序选择
       - StartMenuAdapter.java  //继承BaseAdapter 右键菜单及监听
         - 通过isGrid = mType == ShowType.GRID判断区分list/gride而共用一个适配器．
       - U.java　//工具类，现在更名为LaunchAppUtil.java
  - frameworks/base/packages/SystemUI/src/com/android/systemui/sql
       - SqliteOpenHelper.java  // 创建数据库/版本升级
       - SqliteOperate.java  // 数据库的增删改查
       - StartMenuDatabaseField.java //索引
　- 备注:
    - 1. sql是SystemUI的数据存储．
    - 2. U.java后期改为SystemUI的UtilSystemUI.java
    - 3. 适配器还需要增强，添加拖动功能．

#### 8.0 StartupMenu关键代码
  - 启动方式(启动FreeForm Stack) 借用TaskBar.
    - U.launchApp(mContext, appInfo.getComponentName());
  - 常用列表(list) / app列表(grid) 代码中切换．
  - 添加弹出和退出动画
    ***
          <style name="ShowDialog" >
            <item name="android:windowEnterAnimation">@anim/dialog_enter</item>
            <item name="android:windowExitAnimation">@anim/dialog_exit</item>
        </style>
    ***

***

### 8.0 任务栏的实现

#### 8.0 任务栏设计
  - 1. 利用原生NavigationBar的基础.
  - 2. 新添加布局 oponthos status bar.
  - 3. 在StatusBar中使用addOpenthosStatusBarLayout替换createNavigationBar.

#### 8.0 任务栏的结构
  - frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/phone/
    - StatusBar.java
      - 1. Oreo中优化了之前版本的PhoneStatusBar改为StatusBar.java
      - 2. 该类可以对通知栏/任务栏/导航栏/进行控制．
        - 初始化则通过mStatusBarWindow(StatusBarWindowView的对象) 
      - 3. 该类可以通过AIDL和ＡMS进行交互．
        - 最小化的实现，显示/隐藏的控制．
          - 1. 最小化主要方法:changeStatusBarIcon(int taskId, ComponentName cmp, boolean keep)
          - 2. 显示/隐藏: setStatusBarVisibility(int visibility) 
    - PhoneStatusBarView.java
      - 任务栏的主要自定义View. 5.1中利用了该类在这次设计中不做处理．
    - NavigationBarView.java
      - 导航栏的主要自定义View. 在之前的版本中是StatusBar中直接通过addView获取．
      - 8.0中通过NavigationBarFragment进行获取．
    - NavigationBarFragment.java
      - 新增类，原生对导航栏的控制主要应用于导航栏的视图变化．(不用了解)
    - StatusBarWindowManager.java
      - SystemUI的管理类．
        - 主要涉及方法: public void add(View statusBarView, int barHeight){}
          - 1. 控制SystemUI的宽度和通知栏宽度一样．
          - 2. 控制位置．Gravity.RIGHT | Gravity.TOP.
    - StatusBarWindoView.java
      - SystemUI布局中最外层的自定义View. 该类继承　FrameLayout.
      - StatusBar中进行初始化
        - 方法:inflateStatusBarWindow(Context context) {}
    - OpenthosStatusBarView.java
      - 新添加的布局的自定义View.
      - 在StatusBar中进行初始化.
      - 方法:() View.inflate(context, R.layout.openthos status bar, null);
      - 该主要实现:
          - 1.获取StatusBar对象：SysUiServiceProvider.getComponent(mContext, StatusBar.class);
          - 2. initView / add listener.
          - 3. show notification panel.

  - 备注:
    - 暂时只了解这些类,有新发现再持续更新．

#### 8.0 SystemUI任务栏的布局

  - frameworks/base/packages/SystemUI/res/layout/
    - super_status_bar.xml
      - 1. include status_bar_expanded.xml
      - 2. 对应自定义View: StatusBarWindowView.
    - status_bar_expanded.xml
      - 1. include openthos_notification_panel
      - 2. 对应: NotificationPanelView
    - status_bar.xml
      - 1. 该布局直接被Gone故不用做任何变动．
      - 2. 对应自定义View: PhoneStatusBarView
    - openthos_status_bar.xml
      - 1. 新增加的布局．
      - 2. 添加：StartupMenu/scroll/input/battery/wifi/volume/notification/date/.
      - 3. 对应自定义View: OpenthosStatusBarView.
    
#### 8.0 任务栏的关键代码
  - 1. 设置View :getOpenthosStatusBarLayoutParams(){}
    - LayoutParams.TYPE_NAVIGATION_BAR.

#### 备注
  - OpenthosStatusBarView 替换了NavigationBar, 故NavigationBarView不能初始化，故要处理空指针．


### 8.0通知栏的实现
  
***

#### 通知栏的设计
  - 在status_bar_expanded.xml 中include 一个新布局.
  - 将之前的qs_panel进行gone掉．
  - 新的布局中保有<NotificationStackScrollLayout/>布局．
  
#### 通知栏的结构
  - frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/phone/
    - NotificationPanelView.java
      - 整个通知栏的主要类．
  - frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/stack/
    - NotificationStackScrollLayout.java
      - 通知消息的主要逻辑．

#### 通知栏的布局
  - frameworks/base/packages/SystemUI/res/layout/
    - openthos_notification_panel.xml
      - 新添加的布局．include 在status_bar_expanded.xml中.
      - 通过线性布局的比例分配控制的布局．
    - status_bar_expanded.xml
      - 去掉：NotificationsQuickSettingsContainer 的布局.
      - 将对需求布局没有作用的进行gone.
    
#### 备注
  - 去掉NotificationsQuickSettingsContainer的布局，相应干掉java代码中的相关部分．

#### 通知栏关键代码
  - 通知栏的触发(思路分析)
    - frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/phone/
      - PhoneStatusBarView.java
        - 继承: PanelBar.java
        - onTouchEvent实现触发.return barConsumedEvent || super.onTouchEvent(event);
        - 执行的super　----> 查看　PanelBar.java
    - frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/phone/
      - PanelBar.java
        - 继承FrameLayout.java
        - onTouchEvent ---> return mPanel == null || mPanel.onTouchEvent(event)
        - 返回值取决: mPanel == null ,  PanelView mPanel;
    - frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/phone/
      - PanelView.java
        - FrameLayout.java
        - onTouchEvent　---> 有方法expand(true) (事件触发的关键方法)
        - public void expand(final boolean animate) {}
          - 该方法为public, 只要可以获取到PanelView对象就可以实现该方法.
          - NotificationPanelView extends PanelView
          - 则只要获取NotificationPanelView的对象即可，事情变的简单．
          
#### [任务栏图标锁定](https://github.com/openthos/systemui-analysis/blob/master/LJH/SystemUI%E4%BB%BB%E5%8A%A1%E6%A0%8F%E7%9A%84%E5%9B%BE%E6%A0%87%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%88%86%E6%9E%90.md)

### 任务栏其它快捷方式的实现

***

#### 输入法
  - /frameworks/base/packages/SystemUI8.1/app/src/main/java/com/android/systemui/dialog/
    - InputMethodDialog.java
    

#### 电源
- /frameworks/base/packages/SystemUI8.1/app/src/main/java/com/android/systemui/dialog/
    - BatterydDialog.java

#### wifi
- /frameworks/base/packages/SystemUI8.1/app/src/main/java/com/android/systemui/dialog/
    - WifiDialog.java

#### 音量
- /frameworks/base/packages/SystemUI8.1/app/src/main/java/com/android/systemui/dialog/
    - VolumeDialog.java

#### 日历
- /frameworks/base/packages/SystemUI8.1/app/src/main/java/com/android/systemui/dialog/
    - CalendarDialog.java
- 任务栏上的日期显示来源于一个自定义View
  - /frameworks/base/packages/SystemUI8.1/app/src/main/java/com.android.systemui.dialog
    - CalendarDisplayView.java
    
#### 持续更新　
