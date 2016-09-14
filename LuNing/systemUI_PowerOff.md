#StartMenu电源键功能实现总结
- 控件位置：DocumentsUI/res/layout-sw720dp-land/start_activity.xml 定义onClick点击事件powerOff（View）;
- powerOff实现在DocumentsUI/src/com/android/documentsui/StartupMenuActivity.java
``` 
    ActivityManagerNative.callPowerSource(mContext);
    finish();
``` 
- core/java/android/app/ActivityManagerNative.java 静态方法callPowerSource(mContext)，Intent跳转到电源界面的Activity，关键代码：
``` 
    Intent intent = new Intent();
    intent.setComponent(new ComponentName("com.android.powersource",
                                          "com.android.powersource.PowerSourceActivity"));
    intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK
                    | Intent.FLAG_ACTIVITY_NO_ANIMATION
                    | Intent.FLAG_ACTIVITY_SINGLE_FULLSCREEN);
    context.startActivity(intent);
  ``` 
- packages/PowerSource/src/com/android/powersource/PowerSourceActivity.java实现按钮的点击事件;
  进入PowerSourceActivity时发送广播隐藏状态栏，PowerSourceActivity onDestroy时发送广播显示状态栏;
  关机、重启、睡眠时执行命令行;
  Runtime.getRuntime().exec("su -c \"/system/xbin/poweroff\"");返回Process对象，Process.getInputStream()命令结果的输出流;
  Runtime.getRuntime()可以取得当前JVM的运行时环境，这也是在Java中唯一一个得到运行时环境的方法，参数是执行的命令：su -c /system/xbin/poweroff，实现关机;
- 隐藏和显示状态栏的广播动态注册和实现在policy/src/com/android/internal/policy/impl/PhoneWindowManager.java
  在onReceive中根据Intent.getAction()实现IStatusBarService.aidl的对应方法，(多次跳转)，最终实现在SystemUI/src/com/android/systemui/statusbar/phone/PhoneStatusBar.java
``` 
    mStatusBarWindow.setVisibility(View.VISIBLE);
    mStatusBarView.setVisibility(View.VISIBLE);
    mNotificationPanel.setPanelShow();
``` 
