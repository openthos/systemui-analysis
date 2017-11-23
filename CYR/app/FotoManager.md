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
    

