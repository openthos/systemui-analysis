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
  - [7.0关于多窗口的比较详细的理论知识](https://blog.csdn.net/guoqifa29/article/details/54863237)
  - code分析
    - core/java/android/app/Activity.java:5890:    
    - core/java/android/app/IActivityManager.aidl
    - AMS同过aidl接口实现在Activity中调用AMS的moveTaskBackwards(..);
    - ActivityManagerNative.getDefault().moveTaskBackwards(getTaskId());
    - ## moveTaskBack的逻辑；
    - core/java/android/view/Window.java
    - 在内部接口中interface WindowControllerCallback中添加moveTaskBack()
    - 在Activity中实现该接口,在moveTaskBack中调用ActivityManagerNative.getDefault.moveTaskBackwards(..).
    - core/java/com/android/internal/widget/DecorCaptionView.java  
    - 在该类中实现点击的miniWindow,   callback.moveTaskBack();
    - 最终的实现是:  
      - AMS中 moveTaskBackwards中 task.reparent方法，将taskId放入另一个stack.
      
### 建立一个Stack BACKGROUND_STACK_ID
  - 这个stack的特点就是 不显示，满足最小化的状态.
  - 在ActivityManager中定义（类比系统的stack）
  - 在AMS和ActivityStack中使用(类比系统的stack)
    
    
    
    
    
    
    
    
    
    
    
    
    
  
    

