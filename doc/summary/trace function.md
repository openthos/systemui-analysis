# 以status bar 为入口，了解其子模块位置，分析子模块代码之间的交互经验
内容：
  - 如何根据某个字段和图片名称快速定位在项目中位置
  - 从status bar 为入口了解整个项目
  
##  根据某个字段和图片名称快速定位其在项目中的位置

   - 进入项目中 frameworks/base/  目录下
     * 图片搜索为： find ./ |grep xxx.png 
     * 字段搜索为： grep -rni "\<xxx\>" *
   - 比如说status bar 对应的xml文件名称为： status_bar.xml
     * find ./ |grep status_bar.xml 结果如下：
```
root@23fd8597edf2:~/dongpeng/lollipop-x86/frameworks/base# find ./ |grep status_bar.xml 
./tools/layoutlib/bridge/resources/bars/status_bar.xml
./packages/SystemUI/res/layout/super_status_bar.xml
./packages/SystemUI/res/layout/status_bar.xml
./packages/SystemUI/res/layout/keyguard_status_bar.xml
root@23fd8597edf2:~/dongpeng/lollipop-x86/frameworks/base# ^C
```

##  以status bar 为入口了解整个systemui 模块
  首先我们已经确定了，status bar 对应布局为packages/SystemUI/res/layout/status_bar.xml 也就是sysytem整个模块的入口
  - vim 打开status_bar.xml布局可以看到各个模块的id,通过 字段搜索  可以查看到各个模块的位置，可以进行深入的了解。
