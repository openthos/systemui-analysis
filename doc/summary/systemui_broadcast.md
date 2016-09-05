# 如何进行跨项目数据传输
内容：
- 了解广播机制
-  如何通过广播进行跨项目数据传输

##  了解广播机制：
广播就好比收音机   有发送方，有接收方， 还需要电池; 清单文件注册就是在装电池 
- 引用csdn 详解： [Android 广播机制](http://blog.csdn.net/huangbiao86/article/details/6668525)

##  如何通过广播进行跨项目数据传输

     * 动态注册
```
MyReceiver receiver = new MyReceiver();  
IntentFilter filter = new IntentFilter();  
filter.addAction("android.intent.MY_BROADCAST");  
registerReceiver(receiver, filter);
``````
     * 发送方核心代码
```
             Intent intentSend = new Intent();
             intentSend.putExtra("XXXX",xxxx); // 发送广播需要带的数据
             intentSend.setAction("清单文件注册广播的名称");
             mContext.sendBroadcast(intentSend);
```
     * 接收方核心代码
```
private BroadcastReceiver mBroadcastReceiver = new BroadcastReceiver() {
         public void onReceive(Context context, Intent intent) {
             if (DEBUG) Log.v(TAG, "onReceive: " + intent);
             String action = intent.getAction();
             if (Intent.ACTION_CLOSE_SYSTEM_DIALOGS.equals(action)) {
                if (isCurrentProfile(getSendingUserId())) {
                   int flags = CommandQueue.FLAG_EXCLUDE_NONE;
                     String reason = intent.getStringExtra("reason");
                     if (reason != null && reason.equals(SYSTEM_DIALOG_REASON_RECENT_APPS)) {
                         flags |= CommandQueue.FLAG_EXCLUDE_RECENTS_PANEL;
                     }
                     animateCollapsePanels(flags);
                 }
             }
             else if (Intent.ACTION_SCREEN_OFF.equals(action)) {
                 mScreenOn = false;
                 notifyHeadsUpScreenOn(false);
                 finishBarAnimations();
                 resetUserExpandedStates();
             }
             else if (Intent.ACTION_SCREEN_ON.equals(action)) {
                 mScreenOn = true;
             }
             else if (ACTION_DEMO.equals(action)) {
                 Bundle bundle = intent.getExtras();
                 if (bundle != null) {
                     String command = bundle.g3364
                     String command = bundle.getString("command", "").trim().toLowerCase();
                     if (command.length() > 0) {
                         try {
                             dispatchDemoCommand(command, bundle);
                         } catch (Throwable t) {
                             Log.w(TAG, "Error running demo command, intent=" + intent, t);
                         }
                     }
                 }
             } else if ("fake_artwork".equals(action)) {
                 if (DEBUG_MEDIA_FAKE_ARTWORK) {
                     updateMediaMetaData(true);
                 }
             } else {
               String apkInfo = intent.getStringExtra("keyInfo");
                 if (action.equals("com.android.documentsui.util.startmenudialog")) {
                     loadDocked(apkInfo);
                 }
                 if (action.equals("com.android.systemui.activitykeyview")) {
                    String pkgName = intent.getStringExtra("rmIcon");
                    set.remove(pkgName);
                 }
             }
         }
     };
```
