## SystemUI 工作总结:
###     1.任务栏控件之--HorizontalScrollView的总结
1.1 location:<br \>
frameworks/base/packages/SystemUI/res/status_bar.xml<br \>
                  frameworks/base/packages/SystemUI# cd src/com/android/systemui/statusbar/phone/PhoneStatusBar.java<br \>
     1.2 需求:在任务栏中显示已锁定和正在运行的应用,由于任务栏长度有限,要全部展示,需要用到HorzontalScrollView控件,代码如下:
<HorizontalScrollView
            android:id="@+id/status_bar_scroll_view"
            ...
            android:layout_width="match_parent"
            android:layout_height="@dimen/status_bar_icon_size_big"
            >

            <LinearLayout 
                android:id="@+id/status_bar_activity_contents"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                ...
                android:orientation="horizontal"
                >

            </LinearLayout>
</HorizontalScrollView>
     1.3 分析: HorizontalScrollView是一个 FrameLayout，这意味着你只能在它下面放置一个子控件(这里的LinearLayout),这个子控件可以
     包含很多数据内容.当用户选择打开或者将应用固定到任务栏的时候,填充的view(FrameLayout)会放入LinearLayout中,此时的Linearlayout
     中存在多个应用的图标.当用户关闭应用或者解除固定的时候,LinearLayout会将该View移除掉,再将剩余的View展示出来,关键代码如下:
            打开或固定:
             LayoutInflater li =(LayoutInflater) mContext.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            PackageManager pm = getPackageManagerForUser(-1);
            ApplicationInfo ai = pm.getApplicationInfo(pkg, 0);
            Drawable pkgicon = pm.getApplicationIcon(ai);
            FrameLayout ll = (FrameLayout) li.inflate(R.layout.statusbar_activity_button, null);
            StatusbarActivity sa = new StatusbarActivity(ActivityManager.NOT_RUNNING_STACK_ID, pkg,
                                                         true, false);
            ActivityKeyView akv = (ActivityKeyView) ll.getChildAt(0);
            akv.setStatusbarActivity(sa);
            akv.setFocusedView(ll.findViewById(R.id.activity_focused));
            ((ImageView)akv).setImageDrawable(pkgicon);
            ll.setVisibility(View.VISIBLE);
            mStatusBarActivities.addView(ll);
           关闭或解除固定
           if (mStatusBarActivities.getChildAt(i) instanceof FrameLayout) {
                ActivityKeyView kbv = getActivityKeyView(i);
                if(kbv.getVisibility() == View.GONE) {
                    mStatusBarActivities.removeView(kbv);
                    //continue;
                }
        
     

     2.任务栏常用app之间距离的调整
<FrameLayout
    		android:id="@+id/statusbar_activity_button"
    		xmlns:android="http://schemas.android.com/apk/res/android"
    		android:layout_width="@dimen/status_bar_icon_size_big"
    		android:layout_height="@dimen/status_bar_icon_size_big"
    		android:paddingStart="@dimen/status_bar_icon_padding_big"
    		android:paddingEnd="@dimen/status_bar_icon_padding_big" />
    	<com.android.systemui.statusbar.policy.ActivityKeyView
        	android:id="@+id/activity_key_view"
        	android:layout_width="38dp"
        	android:layout_height="match_parent"
        	android:scaleType="centerInside" />
    	<View
        	android:id="@+id/activity_focused"
        	android:layout_width="match_parent"
        	android:layout_height="match_parent"
        	android:background="#66ffffff"
        	android:visibility="gone"/>
</FrameLayout>
  布局加载和代码添加的比较:
      Framelayout 使用layout_Margin属性的时候,不显示效果.调查发现,原因如下:
      1.framelayout 是在java代码里动态添加的,一般加载布局的使用的是setContentView(R.layout.framelayout)
        setContentView方法会逐行加载xml文件,所以使用该方法,设置的属性会全部读取.
        而在代码里动态添加View的时候,会自适应布局,所以你添加的属性不一定全部有效,所以属性限制的时候,应该具体到控件中.
