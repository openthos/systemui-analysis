### 关于关闭StartupMenu的代码逻辑
***

#### 正常关闭应用
  - ActivityManagerService.java  ---- >  closeActivity(int stackId)
  
#### StartupMenu的关闭
  - ActivityManagerNative.getDefault().killStartupMenu()
  - core/java/android/app/IActivityManager.java   接口 ---- > public boolean killStartupMenu() throws RemoteException;
  - services/core/java/com/android/server/am/ActivityStackSupervisor.java --- > killStartupMenu() (实现部分)
  -  killStartupMenu() (实现部分)   内部调用的还是: closeActivity
