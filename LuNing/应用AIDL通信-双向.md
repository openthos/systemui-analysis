# 双向AIDL
## 服务端
- 1.增加ITest.aidl用于服务端向客户端调用，
```
    LOCAL_SRC_FILES := $(call all-java-files-under, src) \
                       src/com/openthos/seafile/ITest.aidl \
                       src/com/openthos/seafile/ISeafileService.aidl \
```
- 2.ISeafileService.aidl增加接口，用于回调，客户端调用setMsg传入已实现的ITest对象，供服务端调用客户端方法
```
    package com.openthos.seafile;
    import com.openthos.seafile.ITest;
    interface ISeafileService {
        void setMsg(ITest i);
    }
```
- 3.SeafileService.java实现setMsg，通过ITest调用客户端sendMsg方法
```
    private ISeafileService.Stub mBinder = new ISeafileService.Stub() {
        public void setMsg (ITest i) {
            try {
                i.sendMsg("ITest is OK");
            } catch (RemoteException e) {
                e.printStackTrace();
            }
        }
    }
```
## 客户端
- 1.Android.mk引入ITest.aidl
- 2.可在MainActivity.java中实现ITest.aidl，并由ISeafileService对象传回服务端
```
    ITest.Stub mITest = new ITest.Stub() {
        @Override
        public void sendMsg(String s) {
            // 服务端传回数据
            Log.d("dddddd", "----------------" + s);
        }
    }
    
    // ISeafileService传回
    try {
        mISeafileService.setMsg(mITest);
    } catch (RemoteException e) {
        e.printStackTrace
```
