## 对比TaskBar 和 StartupMenu
  - 1. 使用的基本方法
    - StartupMenu： 直接是集成在任务栏上，点击StartupMenu图标就可以弹出开始菜单，然后进行应用的选择打开，再次点击关闭。
    - TaskBar： TaskBar所具有的功能更多一点，你可以对其进行设置，界面更加美化。具体的设置内容  
    
||设置选项|功能|
|---|---|---|
|General settings|Start menu layout|设置start menu 展现的排列模式：list or grid|
||Position on screen|设置start menu所在的位置以及方向（共8个方向）|
||Sear bar visibility|是否显示搜索框以及设置输入键盘的类型（虚拟键盘和实际键盘）|
||configure apps in start menu|设置应用展示的方式（过滤应用和置顶应用）|
||Show scrollbar on start menu|设置Start menu右侧导航条的展示方式|
||Collapse TaskBar when selecting items||
||Automatically collapse TaskBar||
||Alternative position for collapse button||
||Anchor Taskbar when screen rotates||
||Launch on boot||
||System notification settings||
||||
|Appearance|Theme|设置Start menu主题颜色（Dark Light）|
||Icon pack||
||Background tint||
||Accent color||
||Hide button while Taskbar is collapsed||
||Use Taskbar logo as start menu icon||
||Show shortcut icon for pinned apps||
||Visual feedback when selecting icons||
||Transparent start menu||
||Use masks for unthemed icons||
||Reset colors to default||
||||
|Recent apps|||
||||
|Freeform mode|||
||||
|Advanced features|||

<br />

  - 2. TaskBar的实现效果展示
  
  <br />
  
  - 3. TaskBar已经具备的功能
    - 1.展示本机安装的应用
    - 2.固定常用应用到任务栏
    - 3.右键弹出菜单对单个应用的操作（固定到任务栏，卸载，查看应用信息等）
    
  <br />
  
  - 4. TaskBar相对StartupMenu优缺点
    - 1.缺少一些应用排序方式（比如按安装时间，使用频率），但是TaskBar可以自定义过滤应用和置顶应用
    - 2.TaskBar弹出右键菜单的时候会TaskBar会隐藏，看起来很空旷
    - 3.TaskBar的右键菜单在鼠标移动的时候没有选中某一项的提示
    - 4.TaskBar比StartupMenu的展示风格更加多样化
  <br /> 
  
  - 5.TaskBar增加相对StartupMenu缺失功能的实现及难易把握
  <br />
  
  - 6.TaskBar源码移植到系统中的大概问题是否好解决．
    - TaskBar源码已经移植到android 7.1 系统上
  <br /> 
  
  - 7.评价 TaskBar效果
    - 1、整体界面用起来很舒畅
