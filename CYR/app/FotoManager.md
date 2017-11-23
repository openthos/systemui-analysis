### FotoManager的逻辑流程
***
#### 项目结构
![代码结构](https://github.com/openthos/systemui-analysis/blob/master/CYR/icon/FotoManager.png)

***

  - ./app/src/main/java/de/k3d/android/androFotoFinder/AndroFotoFinderApp.java
    - 这个是程序的主要入口
    - extends Application
    - 这里做了一些初始化
      - FotoSqlBase.init();
      - MapsForgeSuport.createInstance(this);
      - ThumNaiUtils.init(this);
    - public void saveToFile() {} 
    
  - /app/src/main/java/de/k3b/android/androFotoFinder/GalleryFilterActivity.java
    - 为全球照片过滤器定义一个界面：只有某些文件路径，日期和/或纬度/经度的照片才可见。
    - GalleryFilterActivity extends LocalizedActivity
    - public static void showActivity(Activity context, IGalleryFilter filter, QueryParameter rootQuery, String lastBookmarkFileName, int resquestCode){}//用于启动Activity
    - 布局文件: activity_gallery_filter.xml
      - 
