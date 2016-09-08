## 1.Settings 模块工作总结
### 1.1 app运行模式选择
       1.location:  
       packages/apps/Settings/src/com/android/settings/RunModeSertings.java             frameWorks/base/packages/OtoSetupWizard/src/com/otosoft/MyProvider.java
       2.介绍：
       界面布局使用ListView展示系统所有app，itm中使用Radiogroup实现运行模式的切换。通过读取数据库，系统为每个应用都
       指定了默认的运行模式，用户可根据需要切换模式运行程序。
       3.开发中遇到的问题 ：
             3.1 由于需要和系统公共的数据库交互，因此将数据库以ContentProvider接口的形式暴露出来，在自定义的
             ContentProvider 中要注意路径匹配者（UriMatcher）添加的路径一定要和配置文件的标识(authority)内容一致，否则，
             别的应用访问不到该接口的数据。关键代码：
                                       
                  <provider  
                       android:authority="com.otosoft.tools.myprovider"
                       android:exported="true"/>

                  final String AUTHORITY="com.otosoft.tools.myprovider";
                  new UriMatcher.addURI(AUTHORITY,"tablename",0);
                       
                          2.对于Radiogroup监听方法的选择。按钮组常用的监听方法(newOnqingkuangCheckedChangedListener(){})
                          操作简便，然而在ListView中使用该监听方法，当item复用的时候会自动触发该监听事件，所以应选择
                          RadioButton按钮的单独监听（new OnClickListener（)）;


###    1.2 app自启管理
        1.location: packages/apps/Settings/src/com/android/settings/AutoStartSettings.java 
        2.介绍：利用ListView显示第三方应用，对app启动方式设置，支持两种设置方式，开机时自动启动和开机时不启动。
        3.开发中遇到的问题 1.Settings模块中，布局基本使用的是Preference及其子类。应用开发布局用的是View及其子类，在Seting
        的界面布局中使用前者，归根到底，Preference的优点在于布局界面的可控性和高效率以及可存储值的简洁性(每个
        PreferenPreferencece存储在相对应下的SharedPreference文件夹下)。Preference布局结构和View布局结构相似，单一控件的
        比较如下：                                

              单一控件：
                        Preference 控件家庭          View控件家庭       控件含义
                          Preference                  TextView           文本框
                          CheckPreference             CheckBox           单选框
                          EditTextPreference          EditText          输入文本框 
                          ListPreference              ListView          列表框
          
           详细了解可参考：http://blog.csdn.net/qinjuning/article/details/6710003

##2.StartupMenu 模块工作总结
###   2.1 app的升降序排序
       1.location：frameworks/base/packages/DocumentUI/src/com/android/documnetui/StartMenuActivity.java
       2.介绍：调用系统应用管理者，获得所有应用的信息（包名，app图标，app名称等），通过GridView将所有app的基本信息展示出来，再对app
       的展示顺序调整。关键代码：

                          PackagerManager pm=activity.getPackagerManager();
                          Intent mainIntent=new Intent(Intent.ACTION_MAIN,null);
                          mainIntent.addCategory(Intent.CATEGORY_LAUNCHER);
                          List list=pm.queryIntentActivities(mainIntent,0);
                          Collection.reverse(list);

       3.开发中遇到的问题 1.点击开始菜单，个别机型存在延迟显示现象。点击开始菜单加载界面的时候，会进行数据库的查询方法，查询为耗时
       操作，致使界面加载变慢，出现延迟。解决思路为，利用广播让数据先于界面提前加载，详细了解可请教本组卢宁。

    总体而言，安卓框架层的代码比较抽象，涉及到的知识面非常广，需要有扎实的Java和安卓基础。和应用开发常用的布局逻辑等区别很大，是对应
    用层的一种提升，需要对核心模块熟练掌握，才能运用自如，这需要一点点的积累！
