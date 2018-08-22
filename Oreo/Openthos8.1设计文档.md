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
  -  数据信息存储数据库SqliteOperateHelper(com.android.systemui.startupmenu.SqliteOperateHelper),此类主要是创建数据库，以及一些增删改查的方法。还有就是查询系统安装应用数据。本类的查询采用单线程池的方式，避免出现数据库死锁的问题
  - 右键菜单以及任务栏弹出的右键菜单类MenuDialog(com.android.systemui.dialog.MenuDialog)
  
#### 监听处理（同时适配鼠标左右键以及不支持鼠标左右键的时候）（com.android.systemui.startupmenu.AppAdapter）
  
  ***
  
        @Override
        public boolean onTouch(View v, MotionEvent event) {
            if (MotionEvent.ACTION_DOWN == event.getAction()) {
                mDownX = (int) event.getRawX();
                mDownY = (int) event.getRawY(); ／／记录点击位置，方便长按事件时使用
                switch (event.getButtonState()) {
                    case MotionEvent.BUTTON_PRIMARY:
                        ／／无论是否支持鼠标右键都会触发的操作，此处不进行逻辑操作，放到onclick事件中进行
                        break;
                    case MotionEvent.BUTTON_SECONDARY:
                        ......
                        return true; //return true后不会再执行onLongClick和onClick监听
                }
            }
            return false;
        }

        @Override
        public void onClick(View v) {
            ／／点击操作
        }

        @Override
        public boolean onLongClick(View v) {
            .....
            return true;   //return true后不会再执行onClick监听
        }
  ***
