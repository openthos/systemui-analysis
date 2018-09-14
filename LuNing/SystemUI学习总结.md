## 学习方法
- 1.浏览整体的类和包，了解大致的代码结构，为下一步阅读文档做准备；
- 2.阅读现有文档，包括5.1和8.1；  
    由于在5.1前期接触过一部分，有一点基础，所以从5.1的文档看起，了解后期所做的改动，出现过的问题；再阅读8.1文档，与5.1比较所做的改动；  
    文档中有整体介绍，也包括重点代码讲解，代码部分可打开对应的类，学习重点代码时，对整个类也要明确作用，与其它类的交互等；便于理清整体结构；  
    对于针对某些问题的文档，可复现后对照原代码和修复代码，分析原因和解决方法；  
    文档：https://github.com/openthos/systemui-analysis/tree/master/LJH
            
- 3.根据罗俊欢整理的55个patch,理解整体流程；  
    patch为frameworks层实现multiwindow和SystemUI基本功能的代码，前12个patch主要为5.1已实现的SystemUI整体代码移植，主要为对界面的重构、基本功能实现、资源文件引入，类结构清晰，重点类不多，后续会做具体介绍；后面的patch包括少部分multiwindow功能完善，以及大部分SystemUI bug修改；
    
- 4.从简单bug入手，深入理解代码；

## 重点类  
- OpenthosStatusBarView.java  
  status整体布局，包括左侧开始菜单按钮，中间应用图标栏，右侧通知栏和home键；  
  控制各个图标的初始化，点击事件，重点为点击事件，弹出对应的菜单；
- StatusBar.java  
  实现应用图标弹出菜单中的对应功能，实现导航栏图标的状态更新和菜单功能；
- dialog package  
  所有dialog所在package;
- startupmenu package
  startupmenu相关类所在package;
