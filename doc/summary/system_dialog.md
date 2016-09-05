# 如何弹出系统级的dialog
内容：
- 为什么需要系统级的dialog
- 如何实现

##  为什么需要系统级的dialog

     * 众所周知 ，普通app开发是基于单窗口模式开发的，弹出的dialog都是基于父布局之内展示的.
     * 如果在多窗口下 需要dialog 和 其父布局并列展示 ，就需要去绘制dialog窗口的位置
     * 保证窗口和父布局窗口一个级别，才能达到并列展示的效果，满足我们的需求.

##  如何实现
  - 首先我们需要继承dialog并且重写 dialog onCreate方法
  ```
  public class BaseSettingDialog extends Dialog {
      protected Context mContext;
      protected View mContentView;
      protected View targetView;
  
      public BaseSettingDialog(Context context) {
          super(context);
          mContext = context;
      }
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
      }
```
  - 接下来在 onCreate 中添加绘制窗口的核心代码

```
          Window window = getWindow();
          window.setType(WindowManager.LayoutParams.TYPE_SYSTEM_ALERT);
          requestWindowFeature(Window.FEATURE_NO_TITLE);
          setCanceledOnTouchOutside(true);
          setCancelable(true);
```
   -  最后当我们需要弹出dialog的时候 继承此类即可 如：</br >
   CalendarDialog extends BaseSettingDialog
