## 获取Apk应用的信息

     File file = new File(reInfo.activityInfo.applicationInfo.sourceDir);
     Date systemDate = new Date(file.lastModified());
     ApplicationInfo applicationInfo = reInfo.activityInfo.applicationInfo;
     String activityName = reInfo.activityInfo.name;
     String pkgName = reInfo.activityInfo.packageName;//包名
     String appLabel = (String) reInfo.loadLabel(pm);
     Drawable icon = reInfo.loadIcon(pm);//图标
     Intent launchIntent = new Intent();
     launchIntent.setComponent(new ComponentName(pkgName, activityName));
     AppInfo appInfo = new AppInfo();
     appInfo.setAppLabel(appLabel);
     appInfo.setPkgName(pkgName);
     appInfo.setDate(systemDate);
     appInfo.setAppIcon(icon);
     appInfo.setIntent(launchIntent);
     mlistAppInfo.add(appInfo);
