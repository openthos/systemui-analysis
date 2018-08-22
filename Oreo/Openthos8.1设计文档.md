## Openthos 8.1 和 Openthos 5.1 SystemUI的设计界面基本相同

### 不同点
 - Openthos 8.1的任务栏以及通知栏现在都是自定义的
 - Openthos 8.1的SystemUI先采用的是以Dialog形式弹出，5.1时使用的是windowManage窗口形式弹出
 - 设计结构上，Openthos 8.1的开始菜单与任务栏的联系更加紧密，去除了5.1时通过广播通信的操作
 - View的展示上，Openthos 8.1的任务栏上多使用的是自定义View，避免了给View设置固定大小，View的大小由任务栏的高度自适应
 
### 目录
  - 开始菜单：com.android.systemui.startupmenu
  - 开始菜单以及任务栏上dialog：com.android.systemui.dialog
  - 开始菜单与任务栏启动公共交互类：com.android.systemui.startupmenu.utils.AppOperateManager
  
### 开始菜单

#### 设计方式
  - 采用dialog中加载自定义View的方式
  - 开始菜单的主体部分是StartupMenuView(com.android.systemui.startupmenu.StartupMenuView),
     弹出方式是在StartupMenuDialog（com.android.systemui.dialog.StartupMenuDialog）,
     启动位置是在OpenthosStatusBarView（com.android.systemui.statusbar.phone.OpenthosStatusBarView）.
  
