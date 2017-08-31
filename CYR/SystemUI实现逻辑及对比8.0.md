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

