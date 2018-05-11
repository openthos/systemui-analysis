
- 与AS中使用放法有不同之处
# 基础AIDL
## 服务端
- 1.清单文件注册service，android:exported="true"可被其它程序访问
```
    <service android:name="com.openthos.seafile.SeafileService"
        android:exported="true">
        <intent-filter>
            <action android:name="com.openthos.seafile.Seafile_Service" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
    </service>
```
- 2.编写ISeafileService.aidl文件
```
    package org.openthos.seafile;
  
    import android.content.pm.ResolveInfo;
  
    interface ISeafileService {
        // eg
        void stopAccount();
        void restoreSettings(boolean wallpaper, boolean wifi,
             boolean appdata, in List<String> syncAppdata, boolean startupmenu,
             boolean browser, in List<String> syncBrowsers, boolean appstore);
    }
```
```
    // 需要在Android.mk中声明
    LOCAL_SRC_FILES := $(call all-java-files-under, src) \
 　　　　　　　　        src/org/openthos/seafile/ISeafileService.aidl
```
- 3.在service中实现aidl方法，并实现绑定方法
```
    // SeafileService.java
    //　实现aidl方法，创建mBinder用于绑定service
    private ISeafileService.Stub mBinder = new ISeafileService.Stub() {
        @Override
        public void stopAccount() {
            ...
        }
        
        @Override
        public void restoreSettings(boolean wallpaper, boolean wifi,
                boolean appdata, List<String> syncAppdata, boolean startupmenu,
                boolean browser, List<String> syncBrowsers, boolean appstore) {
            ...
        }
    }
    
    // 绑定
    @Override
    public IBinder onBind(Intent intent) {
        return mBinder;
    }
```
