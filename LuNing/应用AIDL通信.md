
- 与AS中使用放法有不同之处
# 基础AIDL
## 服务端

- １.编写ISeafileService.aidl文件，放在src目录下与其它java文件同级
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
- ２.在Android.mk中引入aidl文件（也可将aidl文件放在与src同级的lib目录下，包名保持与java文件一致，mk文件引入lib目录即可）
```
    // 需要在Android.mk中声明
    LOCAL_SRC_FILES := $(call all-java-files-under, src) \
 　　　　　　　　        src/org/openthos/seafile/ISeafileService.aidl
```
- ３.清单文件注册service，android:exported="true"可被其它程序访问
```
    <service android:name="com.openthos.seafile.SeafileService"
        android:exported="true">
        <intent-filter>
            <action android:name="com.openthos.seafile.Seafile_Service" />
            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
    </service>
```
- ４.在service中实现aidl方法，并实现绑定方法
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
## 客户端
- 1.在Android.mk中引入服务端的aidl文件
```
    // 方法一：直接使用相对路径引入
    LOCAL_SRC_FILES := \
        $(call all-java-files-under, src) \
        src/com/android/settings/EventLogTags.logtags \
        ../OtoCloudService/src/org/openthos/seafile/ISeafileService.aidl
         
    // 方法二：将服务端的aidl文件拷贝到客户端目录下
    LOCAL_SRC_FILES := $(call all-java-files-under, src) \
        src/com/android/systemui/EventLogTags.logtags \
        src/org/openthos/seafile/ISeafileService.aidl
```
- 2.与服务端绑定，
bindService为异步任务，可能还没绑定，就调用mISeafileService会空指针，因此尽量不要在onCreate中bind的同时，在onCreate中调用mISeafileService
```
    SeafileServiceConnection mSeafileServiceConnection = new SeafileServiceConnection();
    Intent intent = new Intent();
    intent.setComponent(new ComponentName("org.openthos.seafile",
            "org.openthos.seafile.SeafileService"));
    getActivity().bindService(intent, mSeafileServiceConnection, Context.BIND_AUTO_CREATE);
    
    private class SeafileServiceConnection implements ServiceConnection {
        public void onServiceConnected(ComponentName name, IBinder service) {
            mISeafileService = ISeafileService.Stub.asInterface(service);
        }
        public void onServiceDisconnected(ComponentName name) {
        }
    }
```
调用服务端方法需catch RemoteException
```
    try {
        mISeafileService.getAppsInfo(tag);
    } catch (RemoteException e) {
        e.printStackTrace();
    }
```
