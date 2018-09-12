## 多窗口学习
***
### resize window
#### 8.1补丁commintId
```
eebc793273fbee736516ee95c393e2ebc6781cf9
```
##### 分析关键流程
  - 1. TaskPositioner.java
```
./services/core/java/com/android/server/wm/TaskPositioner.java
```
  - 2. 内部类
    - class WindowPositionerEventReceiver extends BatchedInputEventReceiver
    - 在ACTION_MOVE中实现鼠标的拖拽
    - mService.mActivityManager.resizeTask(...);
  - 3. DimLayer.java
    - /services/core/java/com/android/server/wm/DimLayer.java
    - 应用同样是在TaskPositioner中，在move监听中 发大缩小窗口
      - mDimLayerForResize.reDraw(mWindowDragBounds);
      - mDimLayerForResize.setBounds(mWindowDragBounds);
 ***
 
## 窗口最小化的实现
***
### status bar上icon的改变
```
4a5bf9dc2ebf0c947360c462fac7251ed4655f20
```
### 分析code
  - 1. changeStatusBarIcon的实现
    - 主要分两个逻辑: AIDL调用和status bar图标的实现
    - AIDL调用
      - core/java/com/android/internal/statusbar/IStatusBar.aidl
      - packages/SystemUI/src/com/android/systemui/statusbar/CommandQueue.java
      - packages/SystemUI/src/com/android/systemui/statusbar/phone/StatusBar.java
      - services/core/java/com/android/server/statusbar/StatusBarManagerService.java
      - services/core/java/com/android/server/statusbar/StatusBarManagerInternal.java
      - services/core/java/com/android/server/am/ActivityManagerService.java
      - 上面的类主要就是围绕AIDL的调用，目的是在AMS中调用changeStatusBarIcon, 实现是在StatusBar中。
      - AMS调用的时机，选择在finishActivity中/setResumedActivityUncheckLocked(...)/removeTask.
      - status bar 改变icon
      - 代码如下： 细节: 两个Map集合在bindIconToTaskId就是 task与icon绑定。
 ```
 public void changeStatusBarIcon(int taskId, ComponentName cmp, boolean keep) {
         if (!keep || cmp == null) {
             iconClose(taskId);
             return;
         }

         if (mIconMap.get(taskId) != null) {
             setFocusedIcon(taskId);
             //setFocused
         } else {
             bindIconToTaskId(taskId, cmp);
         }
     }
 ```
 ```
 StatusBarManagerInternal statusBarManager =
                                    LocalServices.getService(StatusBarManagerInternal.class);
 ```
#### 8.1最小化的实现逻辑
  - 
    

