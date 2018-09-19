*主要涉及的几个类和主要处理的方法：*
***
    PhoneStatusBar.java  extends  BaseStatusBar.java(抽象类)  
                         implements CommandQueue.Callbacks(Callbacks是一个内部接口)
                        extends IStatusBar.Stub(这是aidl, 这个我成为客户端， 服务端是：IStatusBarService.aidl) 
                        这部分属于实现部分，当然还有另一部分是控制部分。
  
    实现部分的操作：
    1. 在IStatusBar.aidl中写要实现的方法： void hideStatusBarView();  //这是隐藏任务栏。
    2. 根据继承关系，Command.java中自然重写方法，但这里：在Callbacks内部接口中定义该方法.
    3. BaseStatusBar.java是抽象类，并没有全部实现接口中方法，故这里不需要实现方法。
    4. PhoneStatusBar.java中则需要进行重写方法，做出具体的实现。
    5. 代码：
          @Override    
           public void hideStatusBarView() {
                mStatusBarWindow.setVisibility(View.GONE);
             } 
      
  ***
      StatusBarManagerService extends IStatusBarService.Stub
       在IStatusBarService中定义hideStatusBar()
       在StatusBarManagerService中做了实现  ： mBar.hideStatusBarView();
       在PhoneWindowManager.java中进行控制：new 一个StatusBarManagerService的对象进行调方法从而实现对任务栏的隐藏。

***

#### 自动隐藏的code
  - WMS中
```
 7729         private void onCheckingStatusBar(int y, DisplayContent dc) {
 7730             DisplayInfo displayInfo = dc.getDisplayInfo();
 7731             if (y >= displayInfo.logicalHeight - getStatusBarHeight(true)) {
 7732                 mStatusBarLock = true;
 7733             } else {
 7734                 mStatusBarLock = false;
 7735             }
 7736 
 7737             if (y < displayInfo.logicalHeight - STATUS_BAR_SKIP_SENSE_HEIGHT) {
 7738                 mStatusBarSkipSense = false;
 7739             }
 7740 
 7741             if ((y < displayInfo.logicalHeight - STATUS_BAR_CHK_HEIGHT) || !mStatusBarAutoHide
 7742                                                   || mStatusBarSkipSense || mLockMachine.mLocked) {
 7743                 // when window is full, move mouse to up hide statusbar.
 7744                 WindowState window = WindowManagerService.this.mCurrentFocus;
 7745                 int barHeight = mContext.getResources().getDimensionPixelSize(
 7746                                          com.android.internal.R.dimen.status_bar_height_real);
 7747                 int screenHeight = dc.getDisplayInfo().logicalHeight;
 7748                 int screenWidth = dc.getDisplayInfo().logicalWidth;
 7749                 if (window != null && mStatusBarAutoHide && y < (screenHeight - barHeight)
 7750                         && (window.getFrameLw().right + window.getFrameLw().left) == screenWidth
 7751                         && (window.getFrameLw().bottom + window.getFrameLw().top) == screenHeight
 7752                         && !window.getOwningPackage().equals(ApplicationInfo.APPNAME_OTO_LAUNCHER)) {
 7753                     hideStatusbarBroadcast();
 7754                 }
 7755                 return;
 7756             }
 7757             showStatusbarBroadcast();
 7758         }
```

##### 调用处也是WMS
```
 7820         @Override
 7821         public void handleMessage(Message msg) {
                  ...
                   case POINTER_EVENT_ACTION_HOVER_MOVE:
 8281                     onCheckingStatusBar(msg.arg2, (DisplayContent)msg.obj);
 8282                     // There are issues to let WMS crash.
 8283                     //onHoverMove(msg.arg1, msg.arg2, (DisplayContent)msg.obj);
 8284                     break;

```
