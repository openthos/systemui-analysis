# 2017-03-28 总结

## 学习android5.1源码aidl相关知识（以IwallpaperManager.Stub为例）

  - WallpaperManager是如何实现调用WallpaperManagerService的函数
    - WallpaperManagerService extends IwallpaperManager.Stub
    - WallpaperManager中内部类Globals中通过
    
      static class Globals extends IwallpaperManagerCallback.Stub{
		     
          Globals(Looper looper) {
          
              IBinder b = ServiceManager.getService(Context.WALLPAPER_SERVICE);
          
              mService = IWallpaperManager.Stub.asInterface(b);
          }
      }
      
       mService =  IWallpaperManager.Stub.asInterface(b),获得 WallpaperManagerService实例，这样WallpaperManager就可以拿到WallpaperManagerService的引用mService。
