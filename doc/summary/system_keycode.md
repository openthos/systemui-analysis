# 如何通过监听keycode值进行事件的处理
内容：
   * 如何接收keycode
   * 如何实现

## 如何接收keycode：
     * 首要我们要在布局中使用 系统自定义好的button view 即：com.android.systemui.statusbar.policy.KeyButtonView
     * 并且在布局中给keycode赋值 如： systemui:keyCode="100003"
     * 在项目中记录keycode值的位置是 ：core/java/android/view/KeyEvent.java
     * 接受keycode值并对view事件处理是phonewindowmenanger </br > 位置为：policy/src/com/android/internal/policy/impl/PhoneWindowManager.java
