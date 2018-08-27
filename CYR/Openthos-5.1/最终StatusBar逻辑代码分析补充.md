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
            }                                                                                                                                                  Rect mini = new Rect(0, 0, 1, 1);
            Rect defaultWindowSize = new Rect(200,200,800,600);
            Rect actualWindowSize = new Rect();                                                                                                                                                                                                             Rect outOfScreen = new Rect();
            if (mIsMini) {
                Iterator iter = mTreeMap.entrySet().iterator();
                while (iter.hasNext()) {                                                                                                                                                                                                                            try {
                        Map.Entry entry = (Map.Entry) iter.next();
                        Integer id = (Integer) entry.getKey();
                        Rect rect = (Rect) entry.getValue();
                        ActivityManagerNative.getDefault().relayoutWindow(id, rect);
                        ActivityManagerNative.getDefault().setFocusedStack(id);
                        mIsMini = false;
                    } catch (RemoteException e) {
                        Log.e("umic", "Maxmize failed", e);
                    }                                                                                                                                                                                                                                           }
                mTreeMap.clear();
            } else {                                                                                                                                                                                                                                            mTreeMap.clear();
                for (int id : stackIdList) {
                    try {
                        mWindowManager.getStackBounds(id, actualWindowSize);
                        if (actualWindowSize.left < metrics.widthPixels &&
                            actualWindowSize.top < metrics.heightPixels) {
                            outOfScreen.left = actualWindowSize.left + 2 * metrics.widthPixels;
                            outOfScreen.top = actualWindowSize.top + 2 * metrics.heightPixels;
                            outOfScreen.right = actualWindowSize.right + 2 * metrics.widthPixels;
                            outOfScreen.bottom = actualWindowSize.bottom + 2 * metrics.heightPixels;
                            Rect r = new Rect(actualWindowSize.left, actualWindowSize.top,
                                              actualWindowSize.right, actualWindowSize.bottom);
                            mTreeMap.put(id, r);
                            ActivityManagerNative.getDefault().saveInfoInStatusbarActivity(id,
                                                                            actualWindowSize);
                            ActivityManagerNative.getDefault().relayoutWindow(id, outOfScreen);
                            mIsMini = true;
                        }
                    } catch (RemoteException e) {
                        Log.e("umic", "Minimize failed", e);
                    }
                }
            }
        } catch (ActivityNotFoundException e) {
            Slog.w(TAG, "No activity to handle assist action.", e);
        }
```
