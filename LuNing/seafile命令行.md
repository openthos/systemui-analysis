## 准备:
  - su -c tar xvf /system/opt/sea.tar.bz -C /data 解压sea目录
  - su -c chmod -R 777 /data/sea (新版本跳过这两步)
  - su -c busybox mkdir -m 777 -p /data/sea/sdcard/seafile 用于挂载DATA本地目录
  - su -c busybox mount --bind /sdcard/seafile /data/sea/sdcard/seafile 挂载，使seaf-cli可操作DATA,GUI操作的DATA在/sdcard/seafile/帐号/DATA
  
## seaf-cli命令:
 - BASE_COMMAND = su -c ./data/sea/proot.sh -b /data/seafile-config:/data/seafile-config seaf-cli 需要proot
 
### 在帐号登录前完成：
  - su -c busybox mkdir -m 777 /data/seafile-config 创建client配置文件夹
  - BASE_COMMAND init -d /data/seafile-config 初始化配置文件
  - BASE_COMMAND start 启动
  
### 帐号登录后：
  
  - BASE_COMMAND create -n DATA -s http://dev.openthos.org/ -u dev01@openthos.org -tk [token] 创建library；
  - BASE_COMMAND sync -l [library id] -d [library本地路径] -s http://dev.openthos.org/ -u dev01@openthos.org -tk [token] 同步library
  - BASE_COMMAND desync -d [library本地路径] 解除同步
  - BASE_COMMAND download -l [library id] -s http://dev.openthos.org/ -u dev01@openthos.org -p 123 下载指定library
  - BASE_COMMAND stop 停止
  - BASE_COMMAND list-remote -s http://dev.openthos.org/ -u dev01@openthos.org -p 123
列出所有资料库的Name和ID，包括帐号的信息（如token);但如果用户在网页端创建了中文名称的library，Name将无法识别,所以现在获取library列表采用官方client源码中的发送网络请求的方式；
  - list-remote正常:
  ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/list_remote.png)
  - list-remote异常：
  ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/list_remote_error.png)
  
### 官方seaf-cli文档：
https://seacloud.cc/group/3/wiki/seafile-cli-manual.md
