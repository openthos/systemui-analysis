### Power Manager 理论文档
***
  - [详细的在线blog](https://www.cnblogs.com/jamboo/articles/6003835.html
***
  - 应用层
    - PowerManager.java
  - framework层
    - PowerManagerService.java
  - HAL层
    - Power.c
  - 内核层
    - kernal/Power
    
      - 应用接口层: PowerManager开发的接口, 应用可以调用PM的接口申请wakelock，唤醒系统，使系统进入睡眠等操作；
      - ramework层: 应用调用PowerManager开放的接口，来对系统进行一些列的操作是在PowerManagerService中完成的，
    PowerManagerService计算系统中和Power相关的计算，是整个电源管理的决策系统。
    同时协调Power如何与系统其它模块的交互，比如亮屏，暗屏，系统睡眠，唤醒等等。
      - HAL层: 该层只有一个power.c文件，该文件通过上层传下来的参数，向/sys/power/wake_lock或者/sys/power/wake_unlock文件节点写数据来与kernel进行通信，
             主要功能是申请/释放锁，维持屏幕亮灭;
    - PowerManagerService详解
      PowerManagerServcie是android系统电源管理的核心服务，它在Framework层建立起一个策略控制方案，
      向下决策HAL层以及kernel层来控制设备待机状态，控制显示屏，背光灯，距离传感器，光线传感器等硬件设备的状态。
      向上提供给应用程序相应的操作接口，比如:听音乐时持续保持系统唤醒，应用通知来临唤醒手机屏幕等场景.
      kernal层: 内核实现电源管理的三个方案包含桑哥部分
        - Kernel/power/：实现了系统电源管理框架机制。
        - Arch/arm(ormips or powerpc)/mach-XXX/pm.c：实现对特定板的处理器电源管理。
        - drivers/power：是设备电源管理的基础框架，为驱动提供了电源管理接口。
### 申请锁 acquire
     - 应用创建锁之后，必须通过acquire申请锁才能持有wakelock锁，才能保证系统处于唤醒状态来使用电力资源。
       PowerManager和PowerManagerService中都有wakelock内部类，在PowerManagerService启动之时便将其注册到Binder服务端，
       其客户端代理调用由PowerManager来完成，故PowerManager中acquire申请锁具体操作实现在服务端PowerManagerService中的acquireWakeLock();

### 释放锁 release 
       - 在应用持有wakelock锁执行完相应的事物之后，要及时调用release()，来执行释放wakelock操作，否则会导致设备保持唤醒，
       迟迟进入不了睡眠状态，严重影响手机功耗。正常情况下，每个wakelock的acquire都应该对应一个release操作，release操作和acquire流程相似。
