### SystemUI实现逻辑
***
#### SystemUI中的关键类
  - 1. BaseStatusBar.java
    - 在android 8.0中该类被去除．5.1中是一个抽象类．
    - 它的Start()方法继承自SystemUI，该方法实现状态栏启动的具体步骤．
  - 2. SystemUI.java(抽象类)
    - SystemUi被SystemUIService调用，SystemUiService继承service.
    - 所以StatusBar也是一个Service.
    - android 8.０中 SystemUI　implements SystemUiServiceProvider
    
  - 3. CommandQueue.Callbacks
    - BaseStatusBar实现CommanQueue.callbacks接口．
    - CommandQueue继承IStatusBar.Stub远程接口.
    - 内部接口 Callbacks　(call back on the main thread)
    - IStatusBar.Stub接口的方法通过CommandQueue的Callbacks接口实现．
    - 所以BaseStatusBar又是IStatusBar.Stub远程的实现类．
    
  - 4. 实现抽象类BaseStatusBar.java的两个子类．
    - 1. PhoneStatusBar.java
      - android 8.０中替换类是　StatusBar.java
    - 2. TableStatusBar.java(已去除)
    
  - 5. 实现SystemUI的类
    - 1. KeyguardViewMediator.java
    - 2. RingtongPlayer.java
    - 3. VolumeUI.java
    - 4. SystemBars.java
    - 5. PowerUI.java
    - 6. StorageNotification.java
    
***
### SystemUI启动
  - SystemUIService是在SystemServer.java中被启动.
    mActivityManagerService.systemReady(new Runnable() { ~}
      startSystemUI(context);
  - SystemUIService的onCreate()调用SystemUIApplication的方法启动
    SystemUI相关组件, startServicesIfNeeded() , 启动各种Service,
    但是它们不是真正的Service, 是继承SystemUI.java这个抽象类, 复写
    start()方法.

### UI组件启动
  - 通过Handler发 mHandler.sendEmptyMessage消息, 分别调用
    了 StartService() 和 continueStartService()两个方法.
    然后, 通过回调 -- >  SystemBars的对象中;
    -- > 调到PhoneStatusBar的start()方法. 同时PhoneStatusBar在
    start()方法里面调用父类BaseStatusBar的start(); 
    NavigationBar 和 QuickSettiingPanel初始化后, 添加到UI中;

    ```SystemBars中关键代码:
      createStatusBarFromConfig() {
            ...
            mStatusBar = (BaseStatusBar) cls newInstance();
            ...
            mStatusBar.start();
      }```

### NavigationBar导航栏
  - PhoneStatusBar类 start()方法里面调用 addNavigationBar();
   -- > prepareNavigationBarView();  最后
      -- > WindowManager 调用 addView将 NavigationBarView添加到UI界面;
      - -> Back  Home  Recent.

  - PhoneStatusBar类中 prepareNavigationBarView()
***

### App与SystemUI交互(通知消息)
  -App通过android.app.NotificationManager的Notify方法 调用NotificationManagerService的
   SystemService的enqueNotificationWithTag()方法,再进入到enqueueNotificationInternal()方法
  - 位置:
    - frameworks/base/core/java/android/app/NotificaitonManager.java
    - ...            /core/java/android/service/notification/NotificationListenerService.java
    - ...           /core/java/com/android/server/notification/NotificationManagerService.java
    - enqueueNotificationInternal()方法, -- > 限制只能提交 50个通知.
    - if (notification.icon != 0) -- > 判断icon是否为空, 若不为空 则有效.
    - 装完毕准备显示到状态栏, 之后就需要将相关的通知消息告诉所有监听者.
    - notifyPostedLocked()中调用到notifyPosted()方法;
    - ...  listener.onNotificationPosted(sbnHolder, rankUpdate);

    - app清除(Cancel)通知
      - 在NotificationManager之后, 经由NotificationManagerService处理和NotificationListenerService
        的子类, 最后的处理方法变成了onNotificationRemoved.

### Android 8.0SystemUI视觉变化(arm版)
  - 1. 通知栏消息长按可以自定义去设置．
  - 2. 通知消息可以分组管理.
  - 3. 通知栏的功能项减少：　移动数据 勿扰　转屏　省电　飞行 
  - 4. 快速设置settings
  
### Android8.0的逻辑变化
  - 目前只是看到：　PhoneStatusBar.java 和　BaseStatusBar.java去除．
  - 增加StatusBar.java 和　CollapsedStatusBarFragment.java
  
