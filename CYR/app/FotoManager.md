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
      - ![布局视图](https://github.com/openthos/systemui-analysis/blob/master/CYR/icon/activity_gallery_filter.png)
      - 代码中监听的处理:
```
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle presses on the action bar items
        switch (item.getItemId()) {
            case R.id.cmd_select_folder:
                openFolderPicker();
                return true;

            case R.id.cmd_select_lat_lon:
                openLatLonPicker();
                return true;
            case R.id.cmd_select_tag:
                openTagPicker();
                return true;
            case R.id.cmd_filter:
                openFilter();
                return true;
            case R.id.cmd_load_bookmark:
                loadBookmark();
                return true;
            case R.id.cmd_sort_date:
                this.mGalleryQueryParameter.setSortID(FotoSql.SORT_BY_DATE);
                reloadGui("sort date");
                return true;
            case R.id.cmd_sort_directory:
                this.mGalleryQueryParameter.setSortID(FotoSql.SORT_BY_NAME);
                reloadGui("sort dir");
                return true;
            case R.id.cmd_sort_len:
                this.mGalleryQueryParameter.setSortID(FotoSql.SORT_BY_NAME_LEN);
                reloadGui("sort len");
                return true;
            case R.id.cmd_sort_location:
                this.mGalleryQueryParameter.setSortID(FotoSql.SORT_BY_LOCATION);
                reloadGui("sort geo");
                return true;
            case R.id.cmd_settings:
                SettingsActivity.show(this);
                return true;
            case R.id.cmd_about:
                AboutDialogPreference.createAboutDialog(this).show();
                return true;
            case R.id.cmd_more:
                new Handler().postDelayed(new Runnable() {
                    public void run() {
                        // reopen after some delay
                        openOptionsMenu();
                    }
                }, 200);
                return true;
            default:
                return super.onOptionsItemSelected(item);
        }

    }
```
    - 显示菜单中各种item
      - protected void showLatlon(boolean noGeoInfo) {调用方法　show (boolean checked, int ... ids)}
      //对可视的菜单项的处理基本就在这里；
    -
