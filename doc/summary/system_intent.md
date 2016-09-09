#  如何隐士启动activity
   - 隐士启动
   - 代码位置： frameworks/base/packages/PowerSource/src/com/android/powersource/PowerSourceActivity.java （100）
```
                     intent intentLock = new Intent("android.intent.action.LOCKNOW");
                     intentLock.addFlags(Intent.FLAG_RUN_FULLSCREEN | Intent.FLAG_ACTIVITY_NEW_TASK
                                         | Intent.FLAG_ACTIVITY_CLEAR_TASK);
                     startActivity(intentLock);

```
   - 通过获取包名 和action 启动activity  
   - 项目中用到的位置：policy/src/com/android/internal/policy/impl/PhoneWindowManager.java （1397）
```
final Intent intent = new Intent();
		 intent.setComponent(new ComponentName("com.android.documentsui", "com.android.documentsui.StartupMenuActivity"));
		 intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_RUN_STARTUP_MENU
		                 | Intent.FLAG_ACTIVITY_CLEAR_TOP);
		mContext.startActivity(intent);
```
