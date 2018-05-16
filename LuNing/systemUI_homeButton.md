
# StatusBar-Home键功能实现总结
- 在布局SystemUI/status_bar.xml中查找到Home键控件为自定义KeyButtonView,属性keyCode="100003"，类似Button的android:onClick=""，区分不同控件的按键事件;
- core/java/android/view/KeyEvent.java用于定义不同KeyButtonView的keyCode，查找到keyCode=“100003”对应变量为KEYCODE_CUSTOMIZE_HOME;
- policy/src/com/android/internal/policy/impl/PhoneWindowManager.java中对按键事件进行处理;按键事件首先通过PhoneWindowManager的interceptKeyBeforeDispatching(WindowState win, KeyEvent event, int policyFlags)方法被拦截，根据int keyCode = event.getKeyCode()，将事件分发到应用层，一些系统事件：HOME,MENU,SEARCH,会在这里做下预处理;
  当keyCode = KEYCODE_CUSTOMIZE_HOME时Handler发送并接受消息，在startHomeManager()方法中做详细实现;
- startHomeManager()方法包括回到桌面和恢复桌面功能实现;
- 将桌面窗口最小化，实现回到桌面。首先获取最近启动的任务
``` 
    List<RecentTaskInfo> recentTasks =mActivityManager.getRecentTasks
                        (Integer.MAX_VALUE, ActivityManager.RECENT_IGNORE_UNAVAILABLE);
``` 
- 获取home activity
``` 
    ActivityInfo homeActivityInfo =
                new Intent(Intent.ACTION_MAIN).addCategory(Intent.CATEGORY_HOME)
                .resolveActivityInfo(mPackageManager, 0);
``` 
- 遍历任务集合，排除home activity，将应用stackId存入集合,代码如下;
``` 
    for (int i = 0; i < numTasks; ++i) {
      final RecentTaskInfo mRecentTaskInfo = recentTasks.get(i);
      // exclude the home activity
      Intent intent = new Intent(mRecentTaskInfo.baseIntent);
      if (mRecentTaskInfo.origActivity != null) {
          intent.setComponent(mRecentTaskInfo.origActivity);
      }
      if (homeActivityInfo != null) {
        if (homeActivityInfo.packageName.equals(
          intent.getComponent().getPackageName())
            && homeActivityInfo.name.equals(intent.getComponent().getClassName())) {
            continue;
        }
      }
     stackIdList.add(id);
    }
``` 
- 根据boolean值isMini判断窗口是否最小化，isMini = false时窗口没有最小化，遍历stackIdList，根据id获取对应窗口位置坐标mWindowManager.getStackBounds(id, actualWindowSize)，并存入TreeMap(用于窗口恢复),将坐标加上屏幕宽高，重新计算应用位置，ActivityManagerNative.getDefault().relayoutWindow(id, outOfScreen)重构窗口，应用被扔出屏幕，并修改isMini值，代码如下;
``` 
    for (int id : stackIdList) {
      try {
        if (actualWindowSize.left < metrics.widthPixels &&actualWindowSize.top < metrics.heightPixels) {
          outOfScreen.left = actualWindowSize.left + metrics.widthPixels;
          outOfScreen.top = actualWindowSize.top + metrics.heightPixels;
          outOfScreen.right = actualWindowSize.right + metrics.widthPixels;
          outOfScreen.bottom = actualWindowSize.bottom + metrics.heightPixels;
          Rect r = new Rect(actualWindowSize.left, actualWindowSize.top,
                                              actualWindowSize.right, actualWindowSize.bottom);
          mTreeMap.put(id, r);
          ActivityManagerNative.getDefault().saveInfoInStatusbarActivity(id,
                                                                            actualWindowSize);
          ActivityManagerNative.getDefault().relayoutWindow(id, outOfScreen);
          mIsMini = true;
        }
      } catch (RemoteException e) {
        Log.e("umic", "Minimize failed", e);
      }
    }
``` 
- 实现恢复桌面窗口，isMini = true时，获取TreeMap中窗口信息，将窗口恢复，代码如下;
```
    Iterator iter = mTreeMap.entrySet().iterator();
    while (iter.hasNext()) {
      try {
        Map.Entry entry = (Map.Entry) iter.next();
        Integer id = (Integer) entry.getKey();
        Rect rect = (Rect) entry.getValue();
        ActivityManagerNative.getDefault().relayoutWindow(id, rect);
        mIsMini = false;
      } catch (RemoteException e) {
        Log.e("umic", "Maxmize failed", e);
      }
    }
    mTreeMap.clear();
```
- core/java/android/app/ActivityManagerNative.java
  上述代码中ActivityManagerNative.getDefault()返回ActivityManagerProxy对象，ActivityManagerProxy类里包含事件处理方法，如saveInfoInStatusbarActivity，relayoutWindow等;
  saveInfoInStatusbarActivity将id和位置信息存入Parcel类，mRemote.transact(STATUSBAR_ACTIVITY_SAVEINFO, data, reply, 0)，发送消息;
  onTransact处理;
