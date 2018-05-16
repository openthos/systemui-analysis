# StartMenu部分功能实现总结

## 1.application右键卸载
- 1.1 location<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/StartupMenuAdapter.java<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/util/StartMenuDialog.java<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/util/MyInstalledReceiver.java<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/StartupMenuActivity.java<br \>
- 1.2 功能介绍<br \>
StartMenu右侧应用右键点击后弹出框内有卸载功能，点击后跳转到应用卸载界面，进行卸载；<br \>
- 1.3 功能实现<br \>
- 卸载按钮点击事件，Intent通过系统接口和应用报名跳转到应用详情界面，代码如下：<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/util/StartMenuDialog.java
``` 
    Uri uri = Uri.parse("package:" + mPkgName);
    Intent intents = new Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS, uri);
    intents.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    mContext.startActivity(intents);
``` 
- 在清单文件静态注册广播(android.intent.action.PACKAGE_REMOVED)，监听到应用卸载后，根据Intent携带的包名信息从数据库删除整条数据;关于广播的介绍请查看董鹏的：[systemui_broadcast.md](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/systemui_broadcast.md)<br \>

## 2.常用软件限制个数8个
- 2.1 location<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/StartupMenuActivity.java<br \>
- 2.2 功能介绍<br \>
StartMenu左侧常用软件限制个数最多8个，按点击数从上到下排列；<br \>
- 2.3 功能实现<br \>
- 从数据库获取应用信息集合后，根据点击次数排序，取点击数最多的前8个应用，展示到常用软件列表；<br \>

## 3.常用软件限制个数8个
- 3.1 location<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/util/MySqliteOpenHelper.java<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/StartupMenuActivity.java<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/StartupMenuUsuallyAdapter.java<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/util/StartMenuUsuallyDialog.java<br \>
- 3.2 功能介绍<br \>
StartMenu左侧常用软件右键点击弹出框，选择从此列表移除，应用被从常用软件列表删除；<br \>
- 3.3 功能实现<br \>
- 在存储应用信息的数据库增加一个字段click(char)，用来记录左侧常用软件被点击次数，代码如下：<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/util/MySqliteOpenHelper.java
``` 
    db.execSQL("create table perpo(_id integer primary key autoincrement, "
                                    + "label char(20), pkname char(100), date char(50), "
                                    + "int char(10), click char(10))");
``` 
- 选择从此列表移除此应用，根据点击listview的下标，获取应用包名，查找数据库，将此应用对应的click值置为0；列表刷新；<br \>

## 4.StartMenu打开延迟bug
- 4.1 location<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/StartupMenuActivity.java<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/util/MySQLReceiver.java<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/util/MyInstalledReceiver.java<br \>
- 4.2 问题分析<br \>
StartupMenu每次关闭都被System.exit(0),整个应用被结束，无法保存数据，再次打开时重新获取数据；开始菜单展示信息需要先向数据库存入应用信息，再读取出来展示到开始菜单，非常耗时;<br \>
- 4.3 解决方法<br \>
- 安装后第一次打开开始菜单，不需要数据库数据，直接从系统获取应用信息，展示到右侧，代码如下：<br \>
frameworks/base/packages/DocumentsUI/src/com/android/documentsui/StartupMenuActivity.java
``` 
    PackageManager pm = this.getPackageManager();
    Intent mainIntent = new Intent(Intent.ACTION_MAIN, null);
    mainIntent.addCategory(Intent.CATEGORY_LAUNCHER);
    List<ResolveInfo> resolveInfos = pm.queryIntentActivities(mainIntent, 0);
``` 
- 通过SharedPreferences存储本地数据判断是否为第一次打开;<br \>
- 在用户可能对开始菜单进行操作时，才填充数据库，监听鼠标移动（setOnHoverListener(this)）到开始菜单界面上，发送广播，向数据库存储应用信息，此时并不影响开始菜单加载；<br \>
``` 
    @Override
        public boolean onHover(View view, MotionEvent motionEvent) {
            int what = motionEvent.getAction();
            int isSql = sharedPreference.getInt("isSql", 0);
            switch(what) {
                case MotionEvent.ACTION_HOVER_ENTER:
                    if (mIsClick == 0) {
                        if (isSql == 0) {
                            Intent i = new Intent();
                            i.setAction("com.android.documentsui.SQLITE_CHANGE");
                            sendBroadcast(i);
                            SharedPreferences.Editor edit = sharedPreference.edit();
                            edit.putInt("isSql", 1);
                            edit.commit();
                        }
                    }
                    break;
            }
            return false;
        }

``` 
- 从系统直接获取应用名称，而不是从数据库获取，这样系统语言环境变化时，应用名称即时更新，不用更新数据库，减少数据库操作；
- 监听新应用的安装，接收到广播后将新应用信息存入数据库，更新数据库信息；
