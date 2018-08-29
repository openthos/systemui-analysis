#### 最初StatusBar一些逻辑流程
***
**布局文件：status_bar.xml**
  - 布局中分析：
  - ①keyCode实现监听器：systemui:keyCode="100003" 这就类似一个执行监听的标识码，在KeyEvent中则对keyCode进行赋值，KeyEvent中则都是底层的源码。然后在KeyButtonView中进行事件的监听。
  - ②KeyButtonView则是一个继承于imageView的自定义View，extends imageView 构造器方法：
    public ButtonView(Context context) {
        super(context);
    }当然，也存在两个，三个的参数。
    - 学习资源：[自定义类继承ImageView 实现多点图片触碰的拖动和缩放](http://hbxflihua.iteye.com/blog/1485032)
    - ③KeyButtonView中有个onTouchEvent事件： 参数event：参数event为手机屏幕触摸事件封装类的对象，其中封装了该事件的所有信息，例如触摸的位置、触摸的类型以及触摸的时间等。该对象会在用户触摸手机屏幕时被创建。
    - 返回值：该方法的返回值机理与键盘响应事件，同样是当已经完整处理了该事件且不希望其他回调方法再次处理时返回true，否则返回false.**
**主要监听事件：
  - 1.屏幕被按下：MotionEvent.ACTION_DOWN如果在应用程序中需要处理屏幕被按下的事件，只需重新该回调方法，然后在方法中进行动作的判断即可。
  - 2.在屏幕中拖动：MotionEvent.ACTION_MOVE该方法还负责处理触控笔在屏幕上滑动的事件，同样是调用MotionEvent.getAction()方法来判断动作值是否为MotionEvent.ACTION_MOVE再进行处理.
  - 3.屏幕被抬起：MotionEvent.ACTION_UP当触控笔离开屏幕时触发的事件，该事件同样需要onTouchEvent方法来捕捉，然后在方法中进行动作判断。当MotionEvent.getAction()的值为MotionEvent.ACTION_UP时，表示是屏幕被抬起的事件。
  - 在每次触发事件后，都要执行不同的sendEvent方法。
  - 例如:

    void sendEvent(int action, int flags, long when) {
        final int repeatCount = (flags & KeyEvent.FLAG_LONG_PRESS) != 0 ? 1 : 0;
        final KeyEvent ev = new KeyEvent(mDownTime, when, action, mCode, repeatCount,
                0, KeyCharacterMap.VIRTUAL_KEYBOARD, 0,
                flags | KeyEvent.FLAG_FROM_SYSTEM | KeyEvent.FLAG_VIRTUAL_HARD_KEY,
                InputDevice.SOURCE_KEYBOARD);

        InputManager.getInstance().injectInputEvent(ev,
                InputManager.INJECT_INPUT_EVENT_MODE_ASYNC);
    }
**通过这个方法进行与KeyEvent的联系，事件触发后则就是对事件的处理.
处理的方法在PhoneWindowManager中：
public long interceptKeyBeforeDispatching(WindowState win, KeyEvent event, int policyFlags) {}
在这个方法中通过KeyEvent和对应的触发View联系起来，完成整个事件触发的过程。**
***
**PhoneWindowManager中的触发：
1.在interceptKeyBeforeDispatching的方法中调用sendWifiManager，执行：**

    public void sendWifiManager() {
        mHandler.removeMessages(MSG_STARTUP_WIFI);
        mHandler.sendEmptyMessage(MSG_STARTUP_WIFI);
    }
**这样通过Handler来发送消息。然后：**

    private class PolicyHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case MSG_STARTUP_WIFI:
                    startWifiManager();
                    break;
                case MSG_STARTUP_SOUND:
                    startSoundManager();
                    break;
            }
        }
    }
***
**收到消息后触发的函数：
import com.android.internal.statusbar.IStatusBarService;**

    void startWifiManager() {
        try {
            IStatusBarService statusbar = getStatusBarService();
            if (statusbar != null) {
                  statusbar.showWifiPanel();
            }
        } catch (RemoteException e) {
        //注册的方式
             Slog.e(TAG, "RemoteException when showing recent apps", e);
             // re-acquire status bar service next time it is needed.
             mStatusBarService = null;
        }
    }

    解释：IStatusBarService是运行与system_server的一个系统服务，它接受操作栏/导航栏的请求并将其转发给BaseStatusBar。
    消息的传递使用的Handler。
***
**启动StartupMenu,它的触发过程和wifi的触发过程一致**

    void startupMenuInternal() {
        try {
            if (ActivityManagerNative.getDefault().killStartupMenu()) {
                return;
            }
        } catch (RemoteException e) {
            Log.e("LADEHUNTER","Exception when try to kill starup menu !!!!",null);
        }

        final Intent intent = new Intent();
        intent.setComponent(new ComponentName("com.android.documentsui", "com.android.documentsui.StartupMenuActivity"));
        intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_RUN_STARTUP_MENU
                        | Intent.FLAG_ACTIVITY_CLEAR_TOP);
        mContext.startActivity(intent);
    }
**通过intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_RUN_STARTUP_MENU |
  Intent.FLAG_ACTIVITY_CLEAR_TOP)通过三种中一个启动模式进入ActivityStatSupervior在这个类中有一个Rect的函数
  根据传入的启动模式来选择绘制的矩形。**

     Rect getInitializingRect(int intentFlags, int displayId, String pkgName) {
        if ((intentFlags & Intent.FLAG_ACTIVITY_RUN_STARTUP_MENU) != 0) {
            DisplayMetrics metrics = mWindowManager.getDisplayMetrics();
            int h = metrics.heightPixels;//高度的调整
            if (h > WINDOW_HEIGHT) {
                return new Rect(0, 0, DisplayMetrics.WINDOW_STARTUP_MENU_WIDTH,
                    mActivityDisplays.get(displayId).mDisplayInfo.logicalHeight
                        - mWindowManager.getStatusbarHeight());
            } else {
                return new Rect(0, 0, DisplayMetrics.WINDOW_STARTUP_MENU_WIDTH_LAND,
                    mActivityDisplays.get(displayId).mDisplayInfo.logicalHeight
                        - mWindowManager.getStatusbarHeight());
            }
        }
    }
**简单介绍 Rect绘图:**
*new Rect(150, 75, 260, 120) *
*左上角的坐标是（150,75），右下角的坐标是（260,120）*
<hr>
**在DisplayMetrics中绘制的矩形：**
<font color="red">**几个重要方法：**</font>

    //获取屏幕高度
    public int getInitWindowHeightPhone() {
        if (is_16_9()) {
            return (int)((float) heightPixels * WINDOW_INIT_PART_HEIGHT_THIN_16_9);
        } else {
            return (int)((float) heightPixels * WINDOW_INIT_PART_HEIGHT_THIN_4_3);
        }
     //获取位置
     private int getDefaultPosX() {
        initPosX += WINDOW_OFFSET_STEP;
        if (initPosX > WINDOW_OFFSET_MAX) {
            initPosX = WINDOW_OFFSET_STEP;
        }
        return initPosX;
    }
    //进行模式选择：手机/桌面
    public Rect getDefaultFrameRect(boolean phoneStyle) {
        int posX = getDefaultPosX();
        int posY = getDefaultPosY();
        int width = phoneStyle ? getInitWindowWidthPhone() : getInitWindowWidthNormal();
        int height = phoneStyle ? getInitWindowHeightPhone() : getInitWindowHeightNormal();
        return new Rect(posX, posY, posX + width, posY + height);
    }
<hr>

### [后期StatusBar重要变动](https://github.com/openthos/systemui-analysis/blob/master/CYR/Openthos-5.1/%E6%9C%80%E7%BB%88StatusBar%E9%80%BB%E8%BE%91%E4%BB%A3%E7%A0%81%E5%88%86%E6%9E%90%E8%A1%A5%E5%85%85.md)





