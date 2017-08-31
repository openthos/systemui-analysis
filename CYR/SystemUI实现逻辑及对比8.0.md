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
