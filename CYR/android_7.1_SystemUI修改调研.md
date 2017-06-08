## Android 7.1

***

#### 控制SystemUI的位置
  - Location:
    - packages/SystemUI/src/com/android/systemui/statusbar/phone/StatusBarWindowManager.java
    - code:
         - mLp.gravity = Gravity.TOP; -
         + mLp.gravity = Gravity.BOTTOM; +
