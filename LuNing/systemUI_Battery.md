＃状态栏系统电量功能实现总结
- 布局文件SystemUI/res/layout/status_bar.xml <include/>复用布局代码
``` 
    <include layout="@layout/system_icons" />
``` 
- system_icons.xml
``` 
    <com.android.systemui.BatteryMeterView android:id="@+id/battery"
        android:layout_height="14.5dp"
        android:layout_width="9.5dp"
        android:layout_marginBottom="@dimen/battery_margin_bottom"/>
``` 
- SystemUI/src/com/android/systemui/statusbar/phone/PhoneStatusBar.java中找出BatteryMeterView后，BatteryMeterView是一个自定义的图标，实现了BatteryController.BatteryStateChangeCallback接口，接收广播，取出电池各项信息，在draw方法，里面有用电池信息来进行判断，更新图标;
- 可以通过BatteryController感知到电量的变化并对剩余电量的值进行修改。BatteryController继承了BroadcastReceiver，定义了IntentFilter并注册了监听：
``` 
    public BatteryController(Context context) {
      mPowerManager = (PowerManager) context.getSystemService(Context.POWER_SERVICE);
      IntentFilter filter = new IntentFilter();
      filter.addAction(Intent.ACTION_BATTERY_CHANGED);
      filter.addAction(PowerManager.ACTION_POWER_SAVE_MODE_CHANGED);
      filter.addAction(PowerManager.ACTION_POWER_SAVE_MODE_CHANGING);
      context.registerReceiver(this, filter);
      updatePowerSave();
    }
``` 
- 在广播的onReceive（）中进行处理：
``` 
    public void onReceive(Context context, Intent intent) {
      final String action = intent.getAction();
      if (action.equals(Intent.ACTION_BATTERY_CHANGED)) {
        mLevel = (int)(100f
                  * intent.getIntExtra(BatteryManager.EXTRA_LEVEL, 0)
                  / intent.getIntExtra(BatteryManager.EXTRA_SCALE, 100));
          mPluggedIn = intent.getIntExtra(BatteryManager.EXTRA_PLUGGED, 0) != 0;
          final int status = intent.getIntExtra(BatteryManager.EXTRA_STATUS,
                             BatteryManager.BATTERY_STATUS_UNKNOWN);
          mCharged = status == BatteryManager.BATTERY_STATUS_FULL;
          mCharging = mCharged || status == BatteryManager.BATTERY_STATUS_CHARGING;
          fireBatteryLevelChanged();
      } else if (action.equals(PowerManager.ACTION_POWER_SAVE_MODE_CHANGED)) {
        updatePowerSave();
      } else if (action.equals(PowerManager.ACTION_POWER_SAVE_MODE_CHANGING)) {
        setPowerSave(intent.getBooleanExtra(PowerManager.EXTRA_POWER_SAVE_MODE, false));
      }
    }
``` 
