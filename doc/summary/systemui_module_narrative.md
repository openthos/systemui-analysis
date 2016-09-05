
# systemui整体模块叙述
   内容：
- systemui 模块结构

## systemui整体模块结构
   systemui 是原生系统中展示系统信息的模块，目前项目对其功能扩展结构如下：
- systemui 
      * status bar      
      * system icon     
      * Notification bar
      * startup menu    

- status bar
     * 即项目中底部的状态栏它包含 system icon , Notification bar , startup menu.
- system icon 
     * 系统信息包含 声音，wifi，电量，时间，home，输入法.
- Notification bar
    * 通知栏包含即使信息的展示 ，飞行模式，截屏，投影 等功能.
- startup menu 
     * 开始菜单包含 系统app的展示，检索app  ，app分类排序， app运行模式，电源操作 关机 重启 睡眠 锁定 等功能.
