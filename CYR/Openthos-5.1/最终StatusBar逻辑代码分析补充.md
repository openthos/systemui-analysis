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
  
