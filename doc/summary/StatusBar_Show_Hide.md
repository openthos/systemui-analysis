*主要涉及的几个类和主要处理的方法：*
***
    PhoneStatusBar.java  extends  BaseStatusBar.java(抽象类)  
                         implements CommandQueue.Callbacks(Callbacks是一个内部接口)
                        extends IStatusBar.Stub(这是aidl, 这个我成为客户端， 服务端是：IStatusBarService.aidl) 
                        这部分属于实现部分，当然还有另一部分是控制部分。
  
    实现部分的操作：
    1. 在IStatusBar.aidl中写要实现的方法： void hideStatusBarView();  //这是隐藏任务栏。
    2. 根据继承关系，Command.java中自然重写方法，但这里：在Callbacks内部接口中定义该方法.
    3. BaseStatusBar.java是抽象类，并没有全部实现接口中方法，故这里不需要实现方法。
    4. PhoneStatusBar.java中则需要进行重写方法，做出具体的实现。
    5. 代码：
          @Override    
           public void hideStatusBarView() {
                mStatusBarWindow.setVisibility(View.GONE);
             } 
      
  ***
      StatusBarManagerService extends IStatusBarService.Stub
       在IStatusBarService中定义hideStatusBar()
       在StatusBarManagerService中做了实现  ： mBar.hideStatusBarView();
       在PhoneWindowManager.java中进行控制：new 一个StatusBarManagerService的对象进行调方法从而实现对任务栏的隐藏。

***
