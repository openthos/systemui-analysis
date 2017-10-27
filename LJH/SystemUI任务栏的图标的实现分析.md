## android 8.0 SystemUI任务栏图标的实现分析
## 主要涉及的方法 
  - public void changeStatusBarIcon(int taskId, ComponentName cmp, boolean keep) 
  - public void bindIconToTaskId(int taskId, ComponentName cmp)
  - private FrameLayout createIconLayout(final ComponentName cmp)
  - public Drawable getPackageIcon(String pkg)
  - public void closeApp(ComponentName componentName) 
  - public void unlocked(ComponentName componentName)
  - public void locked(ComponentName componentName)
  - public boolean isLocked(ComponentName componentName)
  - public void iconClose(int taskId)
