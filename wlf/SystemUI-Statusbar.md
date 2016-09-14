## SystemUI 工作总结:
###     1.任务栏控件之--HorizontalScrollView的总结
1.1 location:<br \>
frameworks/base/packages/SystemUI/res/status_bar.xml<br \>
                  frameworks/base/packages/SystemUI# cd src/com/android/systemui/statusbar/phone/PhoneStatusBar.java<br \>
 1.2 需求:<br \>
 在任务栏中显示已锁定和正在运行的应用,由于任务栏长度有限,要全部展示,需要用到HorzontalScrollView控件,代码如下:<br \>
     
                        <HorizontalScrollView
                             android:id="@+id/status_bar_scroll_view"
                             ...
                             android:layout_width="match_parent"
                             android:layout_height="@dimen/status_bar_icon_size_big">

                            <LinearLayout 
                                android:id="@+id/status_bar_activity_contents"
                                android:layout_width="match_parent"
                                android:layout_height="match_parent"
                                ...
                                android:orientation="horizontal">

                            </LinearLayout>
                        </HorizontalScrollView>
  1.3 分析:<br \>
  HorizontalScrollView是一个 FrameLayout，这意味着你只能在它下面放置一个子控件(这里的LinearLayout),这个子控件可以   包含很多数据内容.当用户选择打开或者将应用固定到任务栏的时候,填充的view(FrameLayout)会放入LinearLayout中,此时的
  Linearlayout中存在多个应用的图标.当用户关闭应用或者解除固定的时候,LinearLayout会将该View移除掉,再将剩余的View展 示出来,关键代码如下:<br \>
  
                         打开或固定:
                         LayoutInflater li =(LayoutInflater) mContext.
                                            getSystemService(Context.LAYOUT_INFLATER_SERVICE);
                         PackageManager pm = getPackageManagerForUser(-1);
                         ApplicationInfo ai = pm.getApplicationInfo(pkg, 0);
                         Drawable pkgicon = pm.getApplicationIcon(ai);
                         FrameLayout ll = (FrameLayout) li.inflate(R.layout.statusbar_activity_button, null);
                         StatusbarActivity sa = new StatusbarActivity(
                                                ActivityManager.NOT_RUNNING_STACK_ID, pkg,true, false);
                         ActivityKeyView akv = (ActivityKeyView) ll.getChildAt(0);
                         akv.setStatusbarActivity(sa);
                         akv.setFocusedView(ll.findViewById(R.id.activity_focused));
                         ((ImageView)akv).setImageDrawable(pkgicon);
                         ll.setVisibility(View.VISIBLE);
                         mStatusBarActivities.addView(ll);
                         关闭或解除固定:
                         if (mStatusBarActivities.getChildAt(i) instanceof FrameLayout) {
                         ActivityKeyView kbv = getActivityKeyView(i);
                             if(kbv.getVisibility() == View.GONE) {
                             mStatusBarActivities.removeView(kbv);
                             //continue;
                             }
                         }     
        
     
##2.任务栏常用app之间距离的调整

