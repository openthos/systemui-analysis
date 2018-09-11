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
 
