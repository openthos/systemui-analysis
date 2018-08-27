## 应用闪退测试
***
#### 应用和闪退log
***
- 快图
```
699 W/WindowManager( 2813): On display=0 Rebuild removed 4 windows but added 3
700 W/WindowManager( 2813): java.lang.RuntimeException: here
701 W/WindowManager( 2813):         at com.android.server.wm.WindowManagerService.rebuildAppWindowListLocked(WindowManagerService.java:8848)
702 W/WindowManager( 2813):         at com.android.server.wm.WindowManagerService.rebuildAppWindowListLocked(WindowManagerService.java:8775)
703 W/WindowManager( 2813):         at com.android.server.wm.WindowManagerService.handleAppTransitionReadyLocked(WindowManagerService.java:9268)
704 W/WindowManager( 2813):         at com.android.server.wm.WindowManagerService.performLayoutAndPlaceSurfacesLockedInner(WindowManagerService.java:10142)
705 W/WindowManager( 2813):         at com.android.server.wm.WindowManagerService.performLayoutAndPlaceSurfacesLockedLoop(WindowManagerService.java:9002)
706 W/WindowManager( 2813):         at com.android.server.wm.WindowManagerService.performLayoutAndPlaceSurfacesLocked(WindowManagerService.java:8944)
707 W/WindowManager( 2813):         at com.android.server.wm.WindowManagerService.executeAppTransition(WindowManagerService.java:4240)
708 W/WindowManager( 2813):         at com.android.server.am.ActivityStack.resumeTopActivityInnerLocked(ActivityStack.java:1551)
709 W/WindowManager( 2813):         at com.android.server.am.ActivityStack.resumeTopActivityLocked(ActivityStack.java:1487)
710 W/WindowManager( 2813):         at com.android.server.am.ActivityStackSupervisor.resumeTopActivitiesLocked(ActivityStackSupervisor.java:2801)
711 W/WindowManager( 2813):         at com.android.server.am.ActivityStackSupervisor.resumeTopActivitiesLocked(ActivityStackSupervisor.java:2790)
712 W/WindowManager( 2813):         at com.android.server.am.ActivityManagerService.forceStopPackageLocked(ActivityManagerService.java:5943)
713 W/WindowManager( 2813):         at com.android.server.am.ActivityManagerService.broadcastIntentLocked(ActivityManagerService.java:16172)
714 W/WindowManager( 2813):         at com.android.server.am.ActivityManagerService.broadcastIntent(ActivityManagerService.java:16539)

715 W/WindowManager( 2813):         at de.robv.android.xposed.XposedBridge.invokeOriginalMethodNative(Native Method)
716 W/WindowManager( 2813):         at de.robv.android.xposed.XposedBridge.handleHookedMethod(XposedBridge.java:360)
717 W/WindowManager( 2813):         at com.android.server.am.ActivityManagerService.broadcastIntent(<Xposed>)

718 W/WindowManager( 2813):         at android.app.ContextImpl.sendBroadcast(ContextImpl.java:1331)
719 W/WindowManager( 2813):         at org.openthos.taskmanager.prevent.framework.util.AlarmManagerServiceUtils.releaseAlarm(AlarmManagerServiceUtils.java:28)
720 W/WindowManager( 2813):         at org.openthos.taskmanager.prevent.framework.util.HideApiUtils.forceStopPackage(HideApiUtils.java:36)
721 W/WindowManager( 2813):         at org.openthos.taskmanager.prevent.framework.SystemHook.forceStopPackage(SystemHook.java:654)
722 W/WindowManager( 2813):         at org.openthos.taskmanager.prevent.framework.ActivityManagerServiceHook$1.run(ActivityManagerServiceHook.java:277)
723 W/WindowManager( 2813):         at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:422)
724 W/WindowManager( 2813):         at java.util.concurrent.FutureTask.run(FutureTask.java:237)
725 W/WindowManager( 2813):         at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$201(ScheduledThreadPoolExecutor.java:152)
726 W/WindowManager( 2813):         at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:265)
727 W/WindowManager( 2813):         at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1112)
728 W/WindowManager( 2813):         at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:587)
729 W/WindowManager( 2813):         at java.lang.Thread.run(Thread.java:818)
```

- 印象笔记
```
 437 W/EN      ( 5975): [AccountInfoNoOp]{Thread-163} - Called on no-op account info
 438 W/System.err( 5975): java.lang.NoSuchMethodException: setThumbPosition [float]
 439 W/System.err( 5975):    at java.lang.Class.getMethod(Class.java:664)
 440 W/System.err( 5975):    at java.lang.Class.getDeclaredMethod(Class.java:626)
 441 W/System.err( 5975):    at com.evernote.ui.widget.SwitchCompatFix.a(SwitchCompatFix.java:41)
 442 W/System.err( 5975):    at com.evernote.ui.widget.SwitchCompatFix.<init>(SwitchCompatFix.java:26)
 443 W/System.err( 5975):    at java.lang.reflect.Constructor.newInstance(Native Method)
 444 W/System.err( 5975):    at java.lang.reflect.Constructor.newInstance(Constructor.java:288)
 445 W/System.err( 5975):    at android.view.LayoutInflater.createView(LayoutInflater.java:607)
 446 W/System.err( 5975):    at android.view.LayoutInflater.createViewFromTag(LayoutInflater.java:743)
 447 W/System.err( 5975):    at android.view.LayoutInflater.rInflate(LayoutInflater.java:806)
 448 W/System.err( 5975):    at android.view.LayoutInflater.rInflate(LayoutInflater.java:809)
 449 W/System.err( 5975):    at android.view.LayoutInflater.rInflate(LayoutInflater.java:809)
 450 W/System.err( 5975):    at android.view.LayoutInflater.inflate(LayoutInflater.java:504)

 451 W/System.err( 5975):    at de.robv.android.xposed.XposedBridge.invokeOriginalMethodNative(Native Method)
 452 W/System.err( 5975):    at de.robv.android.xposed.XposedBridge.handleHookedMethod(XposedBridge.java:360)
 453 W/System.err( 5975):    at android.view.LayoutInflater.inflate(<Xposed>)

 454 W/System.err( 5975):    at android.view.LayoutInflater.inflate(LayoutInflater.java:414)
 455 W/System.err( 5975):    at com.evernote.ui.NoteListFragment.aX(NoteListFragment.java:6357)
 456 W/System.err( 5975):    at com.evernote.ui.NoteListFragment.a(NoteListFragment.java:3898)
 457 W/System.err( 5975):    at com.evernote.ui.NoteListFragment.onCreateView(NoteListFragment.java:1545)
 458 W/System.err( 5975):    at android.support.v4.app.Fragment.performCreateView(Fragment.java:2192)
 459 W/System.err( 5975):    at android.support.v4.app.FragmentManagerImpl.a(FragmentManager.java:1299)
 460 W/System.err( 5975):    at android.support.v4.app.FragmentManagerImpl.c(FragmentManager.java:1528)
 461 W/System.err( 5975):    at android.support.v4.app.FragmentManagerImpl.a(FragmentManager.java:1595)
 462 W/System.err( 5975):    at android.support.v4.app.FragmentManagerImpl.n(FragmentManager.java:2900)
 463 W/System.err( 5975):    at android.support.v4.app.FragmentController.g(FragmentController.java:201)
 464 W/System.err( 5975):    at android.support.v4.app.FragmentActivity.onStart(FragmentActivity.java:603)
 465 W/System.err( 5975):    at android.support.v7.app.AppCompatActivity.onStart(AppCompatActivity.java:178)
 466 W/System.err( 5975):    at com.evernote.ui.BetterFragmentActivity.onStart(BetterFragmentActivity.java:394)
 467 W/System.err( 5975):    at com.evernote.ui.EvernoteFragmentActivity.onStart(EvernoteFragmentActivity.java:280)
 468 W/System.err( 5975):    at com.evernote.ui.tablet.TabletMainActivity.onStart(TabletMainActivity.java:2699)
 469 W/System.err( 5975):    at android.app.Instrumentation.callActivityOnStart(Instrumentation.java:1236)
 470 W/System.err( 5975):    at android.app.Activity.performStart(Activity.java:6244)
 471 W/System.err( 5975):    at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2288)
 472 W/System.err( 5975):    at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:2387)
 473 W/System.err( 5975):    at android.app.ActivityThread.access$800(ActivityThread.java:151)
 474 W/System.err( 5975):    at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1303)
 475 W/System.err( 5975):    at android.os.Handler.dispatchMessage(Handler.java:102)
 476 W/System.err( 5975):    at android.os.Looper.loop(Looper.java:135)
 477 W/System.err( 5975):    at android.app.ActivityThread.main(ActivityThread.java:5254)
 478 W/System.err( 5975):    at java.lang.reflect.Method.invoke(Native Method)
 479 W/System.err( 5975):    at java.lang.reflect.Method.invoke(Method.java:372)
 480 W/System.err( 5975):    at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:904)
 481 W/System.err( 5975):    at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:699)
 482 W/System.err( 5975):    at de.robv.android.xposed.XposedBridge.main(XposedBridge.java:107)
 483 W/EN      ( 5975): [AccountInfoNoOp]{main} - Called on no-op account info
```
- 学堂在线
```
306 V/WindowManager( 2813): ++++xiej, 04-13,7:26, getOrientationFromAppTokensLocked
307 V/Prevent ( 2813): pid 6616 is not for com.xuetangx.mobile
308 D/Prevent ( 2813): action: destroy activity, package: com.xuetangx.mobile, count: 0
309 D/Prevent ( 2813): action: destroyssss activity, will force stop com.xuetangx.mobile if needed in 6s
310 D/TaskPersister( 2813): removeObsoleteFile: deleting file=138_task.xml
311 D/TaskPersister( 2813): removeObsoleteFile: deleting file=137_task_thumbnail.png
312 D/Prevent ( 2813): checking services, packages: [com.xuetangx.mobile], whitelist: []
313 V/Prevent ( 2813): services size: 24
314 V/Prevent ( 2813): prevents: true, name: com.xuetangx.mobile, count: 0, label: 0, uid: 10073, pid: 6653, process: com.xuetangx.mobile:bdservice_v1, flags: 1
315 V/Prevent ( 2813): prevents: false, name: org.openthos.seafile, count: 1, label: 0, uid: 10000, pid: 3788, process: org.openthos.seafile, flags: 1
316 V/Prevent ( 2813): prevents: false, name: com.github.openthos.printer.localprint, count: 0, label: 0, uid: 10020, pid: 2914, process: com.github.openthos.printer.localpr    int, flags: 1
317 V/Prevent ( 2813): prevents: false, name: com.android.exchange, count: 0, label: 0, uid: 10027, pid: 4290, process: com.android.exchange, flags: 1
318 V/Prevent ( 2813): prevents: false, name: com.tencent.mm, count: 1, label: 0, uid: 10060, pid: 3604, process: com.tencent.mm:push, flags: 1
319 V/Prevent ( 2813): prevents: false, name: com.android.systemui, count: 0, label: 0, uid: 10017, pid: 2950, process: com.android.systemui, flags: 9
320 V/Prevent ( 2813): prevents: false, name: org.openthos.privacyman, count: 0, label: 0, uid: 10036, pid: 4055, process: org.openthos.privacyman, flags: 1
321 V/Prevent ( 2813): prevents: false, name: jackpal.androidterm, count: 1, label: 0, uid: 10010, pid: 4692, process: jackpal.androidterm, flags: 3
322 V/Prevent ( 2813): prevents: false, name: com.tencent.mobileqq, count: 2, label: 0, uid: 10065, pid: 3653, process: com.tencent.mobileqq:MSF, flags: 1

323 I/Prevent ( 2813): com.xuetangx.mobile has running services, force stop it
324 I/Xposed  ( 2813): [I/Prevent] com.xuetangx.mobile has running services, force stop it
325 V/Prevent ( 2813): complete checking running service

326 W/ContextImpl( 2813): Calling a method in the system process without a qualified user: android.app.ContextImpl.sendBroadcast:1327 org.openthos.taskmanager.prevent.framew    ork.util.AlarmManagerServiceUtils.releaseAlarm:28 org.openthos.taskmanager.prevent.framework.util.HideApiUtils.forceStopPackage:36 org.openthos.taskmanager.prevent.frame    work.SystemHook.forceStopPackage:657 org.openthos.taskmanager.prevent.framework.SystemHook$3.run:310

```

- 模拟炒股
```
 173 W/WindowManager( 2813): android.view.InflateException: Binary XML file line #35: Error inflating class <unknown>
 174 W/WindowManager( 2813):         at android.view.LayoutInflater.createView(LayoutInflater.java:633)
 175 W/WindowManager( 2813):         at com.android.internal.policy.impl.PhoneLayoutInflater.onCreateView(PhoneLayoutInflater.java:55)
 176 W/WindowManager( 2813):         at android.view.LayoutInflater.onCreateView(LayoutInflater.java:682)
 177 W/WindowManager( 2813):         at android.view.LayoutInflater.createViewFromTag(LayoutInflater.java:741)
 178 W/WindowManager( 2813):         at android.view.LayoutInflater.rInflate(LayoutInflater.java:806)
 179 W/WindowManager( 2813):         at android.view.LayoutInflater.inflate(LayoutInflater.java:504)

 180 W/WindowManager( 2813):         at de.robv.android.xposed.XposedBridge.invokeOriginalMethodNative(Native Method)
 181 W/WindowManager( 2813):         at de.robv.android.xposed.XposedBridge.handleHookedMethod(XposedBridge.java:360)
 182 W/WindowManager( 2813):         at android.view.LayoutInflater.inflate(<Xposed>)

 183 W/WindowManager( 2813):         at android.view.LayoutInflater.inflate(LayoutInflater.java:414)
 184 W/WindowManager( 2813):         at android.view.LayoutInflater.inflate(LayoutInflater.java:365)
 185 W/WindowManager( 2813):         at com.android.internal.policy.impl.PhoneWindow.generateLayout(PhoneWindow.java:4685)
 186 W/WindowManager( 2813):         at com.android.internal.policy.impl.PhoneWindow.installDecor(PhoneWindow.java:4762)
 187 W/WindowManager( 2813):         at com.android.internal.policy.impl.PhoneWindow.getDecorView(PhoneWindow.java:2051)
 188 W/WindowManager( 2813):         at com.android.internal.policy.impl.PhoneWindowManager.addStartingWindow(PhoneWindowManager.java:2571)
 189 W/WindowManager( 2813):         at com.android.server.wm.WindowManagerService$H.handleMessage(WindowManagerService.java:7900)
 190 W/WindowManager( 2813):         at android.os.Handler.dispatchMessage(Handler.java:102)
 191 W/WindowManager( 2813):         at android.os.Looper.loop(Looper.java:135)
 192 W/WindowManager( 2813):         at android.os.HandlerThread.run(HandlerThread.java:61)
 193 W/WindowManager( 2813):         at com.android.server.ServiceThread.run(ServiceThread.java:46)
 194 W/WindowManager( 2813): Caused by: java.lang.reflect.InvocationTargetException
 195 W/WindowManager( 2813):         at java.lang.reflect.Constructor.newInstance(Native Method)
 196 W/WindowManager( 2813):         at java.lang.reflect.Constructor.newInstance(Constructor.java:288)
 197 W/WindowManager( 2813):         at android.view.LayoutInflater.createView(LayoutInflater.java:607)
 198 W/WindowManager( 2813):         ... 19 more
 199 W/WindowManager( 2813): Caused by: java.lang.RuntimeException: Failed to resolve attribute at index 0
 200 W/WindowManager( 2813):         at android.content.res.TypedArray.getDrawable(TypedArray.java:747)
 201 W/WindowManager( 2813):         at android.content.res.XResources$XTypedArray.getDrawable(XResources.java:1363)
 202 W/WindowManager( 2813):         at android.widget.FrameLayout.<init>(FrameLayout.java:123)
 203 W/WindowManager( 2813):         at android.widget.FrameLayout.<init>(FrameLayout.java:111)
 204 W/WindowManager( 2813):         at android.widget.FrameLayout.<init>(FrameLayout.java:107)
```

- 微博
```
 226 I/ExcludingStoppedHook( 2813): actionandroid.intent.statusbar.hide
 227 E/Native_X( 9626): [elfHookFunction]Not found execv.
 228 W/System.err( 9626): java.io.FileNotFoundException: boot.jar
 229 W/System.err( 9626):    at android.content.res.AssetManager.openAsset(Native Method)
 230 W/System.err( 9626):    at android.content.res.AssetManager.open(AssetManager.java:313)
 231 W/System.err( 9626):    at android.content.res.AssetManager.open(AssetManager.java:287)
 232 W/System.err( 9626):    at com.sina.weibo.bundlemanager.j$a.a(WBBundle.java:522)
 233 W/System.err( 9626):    at com.sina.weibo.bundlemanager.j.a(WBBundle.java:105)
 234 W/System.err( 9626):    at com.sina.weibo.bundlemanager.i.e(ModuleManager.java:331)
 235 W/System.err( 9626):    at com.sina.weibo.bundlemanager.i.a(ModuleManager.java:230)
 236 W/System.err( 9626):    at com.sina.weibo.WeiboApplication.attachBaseContext(WeiboApplication.java:271)
 237 W/System.err( 9626):    at android.app.Application.attach(Application.java:181)
 238 W/System.err( 9626):    at android.app.Instrumentation.newApplication(Instrumentation.java:996)
 239 W/System.err( 9626):    at android.app.Instrumentation.newApplication(Instrumentation.java:980)
 240 W/System.err( 9626):    at android.app.LoadedApk.makeApplication(LoadedApk.java:558)
 241 W/System.err( 9626):    at android.app.ActivityThread.handleBindApplication(ActivityThread.java:4526)
 
 242 W/System.err( 9626):    at de.robv.android.xposed.XposedBridge.invokeOriginalMethodNative(Native Method)
 243 W/System.err( 9626):    at de.robv.android.xposed.XposedBridge.handleHookedMethod(XposedBridge.java:360)
 244 W/System.err( 9626):    at android.app.ActivityThread.handleBindApplication(<Xposed>)
 
 245 W/System.err( 9626):    at android.app.ActivityThread.access$1500(ActivityThread.java:151)
 246 W/System.err( 9626):    at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1364)
 247 W/System.err( 9626):    at android.os.Handler.dispatchMessage(Handler.java:102)
 248 W/System.err( 9626):    at android.os.Looper.loop(Looper.java:135)
 249 W/System.err( 9626):    at android.app.ActivityThread.main(ActivityThread.java:5254)
 250 W/System.err( 9626):    at java.lang.reflect.Method.invoke(Native Method)
 251 W/System.err( 9626):    at java.lang.reflect.Method.invoke(Method.java:372)
 252 W/System.err( 9626):    at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:904)
 253 W/System.err( 9626):    at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:699)
 254 W/System.err( 9626):    at de.robv.android.xposed.XposedBridge.main(XposedBridge.java:107)
 255 E/cutils-trace( 9650): Error opening trace file: Permission denied (13)

```

#### 特点
  - 从应用商店下载安装的应用多数出现闪退;例如：
    - 快图、开心消消乐、微博、印象笔记、cut the rope experiments free、模拟炒股、Skype(登录帐号)、央视影音、哔哩哔哩、学堂在线、影梭；
  - 随系统预装的应用没有闪退：
    - QQ 微信 网易云音乐

  - log的异常多数与Xposed有关；
  - 系统自研应用没有问题；
  - 没有xposed镜像，但是没有安装xposed_installer.apk没有闪退问题；
