### SystemUI实现逻辑
***
#### SystemUI中的关键类
  - 1. BaseStatusBar.java
    - 在android 8.0中该类被去除．5.1中是一个抽象类．
    - 它的Start()方法继承自SystemUI，该方法实现状态栏启动的具体步骤．
  - 2. SystemUI.java
    - SystemUi被SystemUIService调用，SystemUiService继承service.
    - 所以StatusBar也是一个Service.
