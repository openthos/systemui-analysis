## Openthos-2.0工作期间StatusBar中重要修改
***
#### 右下角Home键的实现
  - location: policy/src/com/android/internal/policy/impl/PhoneWindowManager.java
  - method: startHomeManager()
  - 分析: 处理home事件，主要需要处理与原生不同之处是－当fm或者其他应用多开时，需要全部最小化.
  - 主要涉及的类:
    - core/java/android/app/ActivityManager.java
    - policy/src/com/android/internal/policy/impl/PhoneWindowManager.java
    - core/java/android/app/IActivityManager.java
    - core/java/android/app/ActivityManagerNative.java
    - services/core/java/com/android/server/am/ActivityManagerService.java
      - 涉及的method: getAllStackInfos();
    - services/core/java/com/android/server/am/ActivityStackSupervisor.java
      - 涉及的method: getAllStackInfosLocked()
  - code:
```
final Context context = mContext;                                                                                                                  final DisplayMetrics metrics = context.getResources().getDisplayMetrics();
            final PackageManager mPackageManager = context.getPackageManager();
            final ActivityManager mActivityManager = (ActivityManager)
                context.getSystemService(Context.ACTIVITY_SERVICE);

            List<Integer> stackIdList = new ArrayList<>();                                                                                                     List<StackInfo> stackInfoList = mActivityManager.getAllStackInfos();
            for (int i = 0; i < stackInfoList.size(); i++) {
                //exclude the launcher and mini window
                if (stackInfoList.get(i).stackId == 0 || (
                    stackInfoList.get(i).bounds.left >= metrics.widthPixels &&                                                                                         stackInfoList.get(i).bounds.top  >= metrics.heightPixels &&
                    stackInfoList.get(i).bounds.right >= metrics.widthPixels &&                                                                                        stackInfoList.get(i).bounds.bottom >= metrics.heightPixels)) {
                    continue;                                                                                                                                      }
                stackIdList.add(stackInfoList.get(i).stackId);
            }                                                                                                                                                  
```

```
ArrayList<StackInfo> getAllStackInfosLocked() {
        ArrayList<StackInfo> list = new ArrayList<StackInfo>();
        for (int displayNdx = 0; displayNdx < mActivityDisplays.size(); ++displayNdx) {
            ArrayList<ActivityStack> stacks = mActivityDisplays.valueAt(displayNdx).mStacks;
            for (int ndx = stacks.size() - 1; ndx >= 0; --ndx) {
                list.add(getStackInfo(stacks.get(ndx)));
            }
        }
        return list;
    }
```
#### 增加keyCode的触发逻辑 
  - KeyEvent.java  定义: KEYCODE_CUSTOMIZE_HOME
  - status_bar.xml 定义: systemui:keycode="1000003"
  - 在PhoneWindowManager中调用

### InputMethod的icon切换
  - packages/SystemUI/src/com/android/systemui/statusbar/phone/PhoneStatusBar.java
  - code:
```
// When monitor input method data change from systemui or settings,
    // update icon on status bar
    private class SettingsObserverInput extends ContentObserver {
        private Context mContext;
        public SettingsObserverInput(Handler handler, Context context) {
            super(handler);
            mContext = context;
        }

        @Override
        public void onChange(boolean selfChange) {
            updateInputIcon();
        }

        public void registerObserver() {
            final ContentResolver cr = mContext.getContentResolver();
            cr.registerContentObserver(
                  Settings.Secure.getUriFor(Settings.Secure.DEFAULT_INPUT_METHOD), false, this);
            cr.registerContentObserver(Settings.Secure.getUriFor(
                  Settings.Secure.SELECTED_INPUT_METHOD_SUBTYPE), false, this);                                                                            }                                                                                                                                                                                                                                                                                                     public void unregisterObserver() {                                                                                                                     mContext.getContentResolver().unregisterContentObserver(this);
        }                                                                                                                                              }
```
## Status bar 的显示/隐藏
  - [show/hide](https://github.com/openthos/systemui-analysis/blob/master/doc/summary/StatusBar_Show_Hide.md)
  
## 窗口穿透bug的解决
  - 前提: 理解WMS
    - WMS是窗口的管理者，它负责窗口的启动、添加和删除，另外窗口的大小和层级也是由WMS进行管理的。
    - 窗口管理的核心成员有DisplayContent、WindowToken和WindowState。
    - [窗口的启动]()
    - [窗口的添加]()
    - [窗口的删除]()
    - [窗口的切换]()
    - 窗口的穿透就是再上层window执行点击事件，然后事件响应到下面的window。
    - 5.1提交的解决commitId: 66c66ca953b688f1a896a9c1147f70765edd871e and e1a72539072b5a961e1b78754a602cbdf1f2db61
    - StackTapPointerEventListener.java
      - onPointerEvent方法，在ACTION_DOWN中执行
        - mService.mH.obtainMessage(H.TAP_OUTSIDE_STACK, (int) mDownX, (int) mDownY,mDcAndMe).sendToTarget();
        - 在window中弹出一个dialog的window, dialog的window超出父window的边界，这时，点击dialog的window进行穿透到后面的window.
        - 解决方案是针对dialog的几种window针对处理，作为判断条件进行拦截。
        - DisplayContent.java实现window的内容区域
```
293     boolean touchExludeRegion(int x, int y) {
294         WindowState win = mService.mCurrentFocus;
295         if (win != null && (
296             win.getAttrs().type == WindowManager.LayoutParams.TYPE_SYSTEM_DIALOG ||
297             win.getAttrs().type == WindowManager.LayoutParams.TYPE_SYSTEM_ALERT ||
298             win.getAttrs().type == WindowManager.LayoutParams.TYPE_PHONE ||
299             win.getAttrs().type == WindowManager.LayoutParams.TYPE_APPLICATION_PANEL)) {
300             for (int i = getWindowList().size() - 1; i >= 0; i--) {
301                 final WindowState wins = getWindowList().get(i);
302                 if (wins != null && wins.getFrameLw().contains(x, y)) {
303                     return true;
304                 }
305             }
306         }
307         return false;
308     }
```
    
    
    
  
  
  
  
  
  
  
