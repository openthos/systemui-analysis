## android 8.0 SystemUI任务栏图标的实现分析
### 主要涉及的方法 
  - public void changeStatusBarIcon(int taskId, ComponentName cmp, boolean keep) 
  - public void bindIconToTaskId(int taskId, ComponentName cmp)
  - private FrameLayout createIconLayout(final ComponentName cmp)
  - public Drawable getPackageIcon(String pkg)
  - public void closeApp(ComponentName componentName) 
  - public void unlocked(ComponentName componentName)
  - public void locked(ComponentName componentName)
  - public boolean isLocked(ComponentName componentName)
  - public void iconClose(int taskId)

### 方法解释
  - public void changeStatusBarIcon(int taskId, ComponentName cmp, boolean keep) 
    - 这个方法是重写AIDL里的方法，在应用关闭，打开，重新获得焦点的时候会运行。
    - 当 (!keep || cmp == null) 为true时，指的是任务为taskId的进程被关闭，反之则是打开或者是重新获得焦点
  - public void iconClose(int taskId)
    - 当任务关闭的时候执行，这里面的逻辑主要是关闭应用后任务栏的图标以及焦点转换
  - public void bindIconToTaskId(int taskId, ComponentName cmp)
    - 这个主要是针对打开应用和重新获得焦点来执行的
  - private FrameLayout createIconLayout(final ComponentName cmp)
    - 当有新任务的时候任务栏上需要有新的布局生成，并且为新生成的布局文件设置监听事件
  - public Drawable getPackageIcon(String pkg)
    - 根据应用包名获得应用icon
  - public void closeApp(ComponentName componentName) 
    - 关闭应用
  - public void unlocked(ComponentName componentName)
    - 解除固定
  - public void locked(ComponentName componentName)
    - 固定到任务栏
  - public boolean isLocked(ComponentName componentName)
    - 判断应用是否被固定到任务栏
### 一些主要的方法实现
  - 如何关闭应用
  ***
        // mShowIcons.get(componentName).getTaskId() 这里表示的是正在运行的任务taskId
        try {
            ActivityManager.getService().removeTask(mShowIcons.get(componentName).getTaskId());
        } catch (Exception e) {
        }
  ***
  
  - 如何使应用重新获得焦点
  ***
        //当应用正在运行的时候调用am.setFocusedTask(mShowIcons.get(cmp).getTaskId())重新获得焦点，
        //反之调用 U.launchApp(mContext, cmp) 打开应用
        try {
            IActivityManager am = ActivityManager.getService();
            if (mShowIcons.get(cmp).isRun()) {
                am.setFocusedTask(mShowIcons.get(cmp).getTaskId());
            } else {
                U.launchApp(mContext, cmp);
            }
        } catch (Exception e) {
            Log.e(TAG, "Error during setFocuesTask", e);
        }
  ***
    
