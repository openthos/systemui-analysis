##Terminal_Emulator 分析:
openthos 项目中,Terminal_Emulator应用不存在源码,以第三方apk的方式存在,源码可以在github下载.具体下载地址为:
https://github.com/jackpal/Android-Terminal-Emulator
<br \>Terminal_Emulator应用在android_x86 平台存在的问题:<br \>
1.以多窗口的形式打开该应用,应用无法识别窗口的下边缘和右边缘;<br \>
问题分析:<br \>
应用主布局路径:<br \>
Android-Terminal-Emulator-master/term/src/main/res/layout/term_activity.xml<br \>
该应用的整体布局为:<br \>
                    <jackpal.androidterm.TermViewFlipper
                    android:id="@+id/view_flipper"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:background="@android:color/black"
                    />
其中自定义的TermViewFilpper继承自ViewFilpper.该控件的用法和ViewPager类似,可实现页面的增加和切换等,ViewFilpper学习可参考:
http://www.android100.org/html/201403/08/5835.html
java代码中可动态给TermViewFilpper添加TermView,代码逻辑如下:
            TermView view = createEmulatorView(session);
            view.updatePrefs(mSettings);
            mViewFlipper.addView(view); 
具体逻辑可参考:
Android-Terminal-Emulator-master/term/src/main/java/jackpal/androidterm/Term.java
这里的TermView就是Terminal_Emulator应用的核心部分,可参考:
Android-Terminal-Emulator-master/term/src/main/java/jackpal/androidterm/TermView.java

当该应用启动的时候,会执行TermView的onDraw(int width, int height)方法,在该方法中,width height为系统获得的TermView的宽和高.在移动设备上,该尺寸即为设备
的宽高,在android_x86设备上,该尺寸也是设备的屏幕尺寸.问题就出在,当应用以多窗口的形式打开的时候,onDraw()方法获取的宽高仍是设备的宽高而不是多窗口实际的宽高.在多窗口的情况下,如何获取窗口的尺寸进行TermView的绘制成为关键,下一步将调研解决如何获取到正确的多窗口尺寸.
