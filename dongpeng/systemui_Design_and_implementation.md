
# Systemui功能需求与设计实现文档
内容：

- 项目简介
- 功能需求
- 存在问题
- 3月需求
- 3月计划
- 项目进展
- 设计实现

# 项目简介

本项目属于openthos项目的一部分，提供 [Openthos](https://github.com/openthos/openthos/wiki) 对原SystemUI状态栏系统信息显示位置重定制，以及对现有的功能扩展，并且追加新的功能开始菜单，和常用软件。

## 当前开发人员 （20170301 - 20170331）
曹永韧

## 当前开发人员 (20160801-20160831)
董鹏 卢宁 曹永韧 王利峰

## 当前开发人员 (20160801-20160831)
董鹏 曹永韧 刘晓旭 卢宁

## 功能需求

|完成|描述|模块|完成度|
|---|---|---|---|
|√| 开始菜单常用软件|界面|100%|
|√| 开始菜单app|界面|100%|
|√| 开始菜单电源|界面|95%|
|√| 开始菜单app分类|界面|90%|
|√| 状态栏时间|界面|100%|
|√| 状态栏常用软件|界面|100%|
|√| 状态栏通知栏重置|界面|90%|
|√| 状态栏输入法扩展|界面|100%|
|√| 状态栏WIFI扩展|界面|100%|
|√| 状态栏声音扩展|界面|100%|
|√| 状态栏对于app锁定|功能|100%|
|√| 状态栏对锁定的app右键弹出Dialog打开应用 解除锁定 手机模式 桌面模式 |功能|100%|
|√| 状态栏对任务栏右键选择显示 or 隐藏 |功能|100%|
|√| 开始菜单常用软件|功能|100%|
|√| 开始菜单展示全部app |功能|100%|
|√| 开始菜单搜索|功能|100%|
|√| 开始菜单三种排序：使用频率，安装时间，名称排序|功能|100%|
|√| 开始菜单左边常用软件鼠标右键|功能|100%|
|√| 开始菜单右边app鼠标右键可运行可卸载|功能|100%|
|√| 开始菜单电源：关机 重启 锁平 |功能|100%|
|×| 开始菜单电源：锁屏|功能|0%|
|√| 开始菜单电源 键盘控制|功能|100%|
|√| 开始菜单我的电脑 设置|功能|100%|
|√| 状态栏常用软件|功能|100%|
|√| 状态栏日历|功能|100%|
|√| 状态栏home|功能|100%|
|√| 状态栏声音|功能|100%|
|√| 状态栏通知栏|功能|100%|
|√| 状态栏输入法|功能|100%|
|√| 状态栏WIFI|功能|100%|
|√| 状态栏电量|功能|100%|
|√| 通知栏清除所有 |功能|100%|
|√| 通知栏截屏 |功能|100%|
|√| 通知栏隔离模式 |功能|100%|
|√| 通知栏设置 |功能| 100%|
|×| 通知栏投影 |功能| 0%|
|√| 通知栏打印管理|功能|100%|

## 17-03存在的问题

| 简述 | 类别 | 备注
|---|---|---|
|通知栏中英文国际化失败|bug|https://dev.openthos.org/zentao/zentao/bug-view-401.html|
|通知栏隔离模式图标不改变|bug|https://dev.openthos.org/zentao/zentao/bug-view-688.html|
|状态栏上的图标不能移动|bug|https://dev.openthos.org/zentao/zentao/bug-view-660.html|
|状态栏日历有时显示不全|bug|https://dev.openthos.org/zentao/zentao/bug-view-1171.html|
|状态栏的自动显示与隐藏屏幕遮盖|功能|https://dev.openthos.org/zentao/zentao/bug-view-752.html|
|开始菜单的延迟需要优化|功能|功能优先级未开始|
|开始菜单app分类|功能|功能优先级未开始|
|状态栏的状态保存|功能|优先级较高已开始|

## 16-08月存在问题

| 简述 | 类别 | 备注
|---|---|---|
|调用系统锁屏功能在同方笔记本T43u 和同方台式机精锐x700无效（待调研）|bug|https://dev.openthos.org/zentao/zentao/bug-view-182.html|
|开始菜单点击电源弹出的dialog无法覆盖全屏(同多窗口有关待调研)|bug|https://dev.openthos.org/zentao/zentao/bug-view-194.html|
|Systemui 状态栏中源WIFI逻辑比较复杂需要调研|bug|https://dev.openthos.org/zentao/zentao/bug-view-100.html|
|Systemui与DocumentsUI 需要进行跨项目数据交互 并变更DocumentsUI数据库的内容 功能难度系数高比较耗时|bug|https://dev.openthos.org/zentao/zentao/bug-view-56.html|
|重置Systemui通知栏|功能|功能优先级未开始，需要底层人员协助|
|扩展Systemui 输入法|功能|功能优先级未开始|
|扩展Systemui 电量|功能|功能优先级未开始|

## 17-03月需求
- 1.任务栏保存显示或隐藏的状态。
- 2.任务栏电源图标的修改。
- 3.通知栏的布局优化。
- 4.任务栏以太网的图标变换bug的解决。
- 5.开始菜单右键Dialog的优化。

## 16-08月需求
- 1.开始菜单字体支持中文英文
- 2.开始菜单页面图片修改
- 3.开始菜单布局修改
- 4.开始菜单电源功能布局 图片修改
- 5.状态栏左边常用软件图片修改
- 6.状态栏右边系统7个信息（通知栏，电量，输入法，wifi，时间 ，声音，home键）图片修改
- 7.开始菜单右边鼠标右键可卸载可运行功能
- 8.开始菜单左边鼠标右键可取消功能
- 9.状态栏 电量 输入法 功能实现

## 16-08月份任务计划
| 时间节点 | 任务 
|---|---|
|2016/08/01-2016-/08/07 |1 .开始菜单字体支持中文英文 <br />2.实现鼠标右键功能|
|2016/08/08-2016-/08/14 |1 开始菜单页面图片修改。 <br />2开始菜单页面图片修改 <br />3 开始菜单布局修改 <br />4 开始菜单电源功能布局 图片修改<br /> 5 状态栏左边常用软件图片修改 <br />6 状态栏右边系统<br />7个信息（通知栏，电量，输入法，wifi，时间 ，声音，home键）图片修改|
|2016/08/15-2016-/08/21 | 1 开始菜单右边鼠标右键可卸载可运行功能<br /> 2 开始菜单左边鼠标右键可取消功能| 
|2016/08/22-2016-/08/26 |一. 图片替换 <br />1.电源,设置,文件管理器 图标大<br /> 2.菜单选项上三角不明显,需重新提供<br />3.鼠标滑过及点击效果,图片一天切换.<br />4.底部任务栏鼠标事件未替换成效果图<br />5.Startmenu-电源 背景黑色太浅 ;home加一个白线 ;<br />6.输入法 ，电量 ，wifi， 声音，时间<br />7.多窗口三个图标： 关闭 最大化 隐藏<br />二. 布局修改 <br />  1 .左侧(常用软件)与右侧(app列表)比例不对,需修改<br /> 2 wifi 和声音 布局 调整 修改背影<br /> 3.-周一确认返回按钮,及位置<br /> 4.startmenu点击效果位置偏移<br />三. 现有功能扩展 <br /> 1.以手机模式运行<br />2.以桌面模式运行<br />  3.从此列表中移除<br />4.分类排序，升序降序<br />5.电源<br /> 6.输入法<br />|

## 16-09月份任务计划
| 时间节点 | 任务 
|---|---|
|2016/09/18-2016-/09/23 |修复禅道bug（共22个）平均每人4-5个 分配如下： <br />1.董鹏 [fix_bug_dp.md](https://github.com/openthos/systemui-analysis/blob/master/dongpeng/fix_bug_dp.md)<br /> 2.卢宁[fix_bug_ln.md](https://github.com/openthos/systemui-analysis/blob/master/LuNing/fix_bug_ln.md)<br />3.曹永韧 [fix_bug_cyr.md](https://github.com/openthos/systemui-analysis/blob/master/CYR/fix_bug_cyr.md)<br />4. 王利峰[fix_bug_wlf.md](https://github.com/openthos/systemui-analysis/blob/master/wlf/fix_bug_wlf.md)|
|2016/09/26-2016-/09/30 |Systemui组 上周解决bug 9个，新增3个，重复bug2个(已关闭)， 目前剩余bug14个 以下为本周任务分配和bug分类综述：<br />本周平均每人3-4个bug 分配如下：<br />1.董鹏 [fix_bug_dp.md](https://github.com/openthos/systemui-analysis/blob/master/dongpeng/fix_bug_dp.md)<br /> 2.卢宁[fix_bug_ln.md](https://github.com/openthos/systemui-analysis/blob/master/LuNing/fix_bug_ln.md)<br />3.曹永韧 [fix_bug_cyr.md](https://github.com/openthos/systemui-analysis/blob/master/CYR/fix_bug_cyr.md)<br />4. 王利峰[fix_bug_wlf.md](https://github.com/openthos/systemui-analysis/blob/master/wlf/fix_bug_wlf.md)<br />Systemui bug 分类综述[bug_classify.md](https://github.com/openthos/systemui-analysis/blob/master/dongpeng/bug_classify.md)<br />

## 16-10月份任务计划
| 时间节点 | 任务 
|---|---|
|2016/10/8-2016-/10/21 |本月主要任务为：优化现有功能，实现8月遗留的function,任务分配如下： <br />1.独立Startmenu （董鹏） <br /> 2.电量 （卢宁）<br />3.通知栏 布局优化/逻辑调整   （曹永韧， 董鹏协助）<br />4. 输入法    （王利峰， 董鹏协助）|
|2016/10/24-2016-/10/28 | <br />1.Startupmenu 最新dpi适配工作 （董鹏） <br /> 2.Systemui 最新dpi适配工作 （卢宁）<br />3.通知栏 最新dpi适配工作  （曹永韧）<br />4. 熟悉openthos项目，掌握基本的vim git 指令  解决1-2 bug（刘晓旭， 董鹏协助）|

## 16-11月份任务计划
| 时间节点 | 任务 
|---|---|
|2016/11/1-2016-/11/30 |本月主要任务为：强化现有功能,解决最新openthos使用发现影响性能优先级高的bug <br />任务分配如下： <br />1.董鹏    [work_plan.md](https://github.com/openthos/systemui-analysis/blob/master/dongpeng/work_plan.md) <br /> 2.曹永韧  [work_plan.md](https://github.com/openthos/systemui-analysis/blob/master/dongpeng/work_plan.md)  <br />3.刘晓旭  [work_plan.md](https://github.com/openthos/systemui-analysis/blob/master/dongpeng/work_plan.md) <br />|


## 17-03月份任务计划
| 时间节点 | 任务 
|---|---|
|2017/03/1-2017/3/31 |本月主要任务为：强化现有功能,解决最新openthos使用发现影响性能优先级高的bug <br />任务分配如下： <br />曹永韧    [work_plan.md](https://github.com/openthos/community-analysis/blob/master/weekly%20report/Weekly_report_about_SystemUI.md) <br /> |


# 项目进展

- 2016/03/21-2016/04/15
  * 董鹏
    * 熟悉Systemui/../starsbar/PhoneStarsbar.java中函数之间的调用关系
    * 在本地实现开始菜单页面原型。 
    * 同 陈工 谢工 冯杰 讨论项目页面开发的切入口。

- 2016/04/16-2016/04/30
  * 董鹏
      * 在mutilwindows实现Systemui状态栏位置的重置。* 在本地实现开始菜单展示所有app的功能
  * 陈刚   
      * 在陈工的带领下把追加的开始菜单模型及其功能第一次集成到mutilwindows中* 使其页面有了初步的可视效果。
- 2016/05/01-2016/05/15
  * 董鹏
    * 完善了开始菜单展示所有app的功能。
    * 添加我的电脑，设置， 电源（关机，重启，睡眠，锁定，搜索，app分类，常用软件的原型页面 。

- 2016/05/16-2016/05/31
  * 董鹏
    * 实现的function 有我的电脑，设置，电源中的关机，重启，和锁定。

- 2016/06/01-2016/06/15
  * 董鹏
    * 实现开始菜单搜索 ，名称排序，使用频率排序

- 2016/06/16-2016-/06/30
  * 董鹏
    * 实现开始菜单时间排序。左边常用软件
    * 添加Systemui 状态栏中： 通知栏 ，电量，声音，wifi,时间 ，home 页面原型及接口。
  * 余明凯
    * 实现Systemui 状态栏home键功能。

- 2016/07/01-2016-/07/15
  * 董鹏
    * 根据优先级解决|bug|https://dev.openthos.org/zentao/zentao/bug-view-39.html|
    * 根据优先级解决|bug|https://dev.openthos.org/zentao/zentao/bug-view-82.html|
    * 实现Systemui状态栏 时间功能
  * 余明凯
    * 实现Systemui 状态栏 声音， WIFI。
- 2016/07/16-2016-/07/31
  * 董鹏
    * 根据最新效果图重置开始菜单页面，及其功能
    * 根据效果图重置开始菜单电源页面，及其展示位置
- 2016/08/01-2016-/08/15
  * 董鹏 卢宁
    * 根据最新效果图重置开始菜单页面，及其功能
    * 根据效果图重置开始菜单电源页面，及其展示位置
- 2016/08/15-2016-/08/21
  * 董鹏 
    * 开始菜单左侧鼠标右键
    * 多窗口阴影调研
  * 卢宁
    * 左侧常用软件列表-限制7个。
    * App应用名称长度限制10个字符.
    * 卸载
  * 曹永韧
    * 固定到任务栏。
  * 王利峰
    * 分类排序，升序降序。
  * 于鹏 
    * 电源。
    * 输入法
- 2016/08/22-2016-/08/26
  * 董鹏 
    * Startmenu-电源 背景黑色太浅 ;home加一个白线
    * 左侧(常用软件)与右侧(app列表)比例不对,需修改
    * wifi 和声音 布局 调整 修改背影
    * 电源
  * 卢宁
    * 多窗口三个图标： 关闭 最大化 隐藏。
    * startmenu点击效果位置偏移
    * 从此列表中移除
    * 输入法
  * 曹永韧
    * 输入法 ，电量 ，wifi， 声音，时间图标替换
    * 以手机模式运行。
    * 以桌面模式运行
  * 王利峰
    * 电源,设置,文件管理器 图标大
    * 分类排序，升序降序。
    * 菜单选项上三角不明显,需重新提供
    * 鼠标滑过及点击效果,图片一天切换
    * 底部任务栏鼠标事件未替换成效果图
    * 周一确认返回按钮,及位置
- 2016/08/27-2016-/08/30
  * 董鹏 
  * bug  修复
  
- 17/03/01 - 17/03/16
  - 曹永韧
  - 锁屏bug 已修复。
  - 休眠因存在问题暂时禁用。
  - 日历的bug 已修复.
  - 以太网的图标变换问题解决。
   
# 设计与实现

分别叙述整体的模块和技术要点。

##　systemui整体模块叙述
- 请查看：[systemui_module_narrative.md](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/systemui_module_narrative.md)

##　以status bar为入口，查看各个模块所在位置。
- 请查看： [trace function.md](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/trace%20function.md)
 
##　如何进行跨项目间的数据传输
 - 项目中之间数据传输是通过有序广播来实现接受和处理的 </br >请查看 ：
 [systemui_broadcast.md](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/systemui_broadcast.md)

##　如何弹系统级的dialog
- systemui apk 整个宽度为不大于100单位值，如果在这里弹出一个dialog是无法看到的，</br >因为其父布局的高度只有不到100单位值，
如何弹出一个同父布局一个等级的dialog：</br >请查看：
[system_dialog.md](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/system_dialog.md)

##  如何通过监听keycode值进行事件的处理
- 一个view 想要处理点击事件  一种是写一个onClick函数 ，一种是实现onClick监听事件
	      今天给大家说一下android中   通过监听keycode值进行事件的处理：</br >请查看：
[system_keycode.md](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/system_keycode.md)

##  如何隐士启动一个activity
- 请查看： [system_intent.md]()
    
##  如何通过标记flg值启动acticity

##  如何实现鼠标右键的功能
请查看：[mouse_right_click.md](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/mouse_right_click.md)
##  如何实现鼠标划过获取对应apk信息
请查看：[get_apk_info.md](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/get_apk_info.md)
##  项目res资源文件调研

## 任务栏显示和隐藏的控制逻辑
请查看：[StatusBar_Show_Hide.md](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/StatusBar_Show_Hide.md)

