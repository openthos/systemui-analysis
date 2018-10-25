## 准备:
  - tar xvf /system/opt/sea.tar.bz -C /data 解压sea目录
  - chmod -R 777 /data/sea (新版本跳过这两步)
  - busybox mkdir -m 777 -p /data/sea/sdcard/seafile 用于挂载DATA本地目录
  - busybox mount --bind /sdcard/seafile /data/sea/sdcard/seafile 挂载，使seaf-cli可操作DATA,GUI操作的DATA在/sdcard/seafile/帐号/DATA
  
## seaf-cli命令（不考虑proot）:
### 在帐号登录前完成：
  - busybox mkdir -m 777 /data/seafile-config 创建client配置文件夹
  - seaf-cli init -d /data/seafile-config 初始化配置文件
  - seaf-cli start 启动
  
### 帐号登录后：
  - seaf-cli list-remote -s http://dev.openthos.org/ -u dev01@openthos.org -p 123
列出所有资料库的Name和ID，包括帐号的信息（如token);但如果用户在网页端创建了中文名称的library，Name将无法识别,所以现在获取library列表采用官方client源码中的发送网络请求的方式；
  - seaf-cli create -n DATA -s http://dev.openthos.org/ -u dev01@openthos.org -tk [token] 如帐号中没有DATA或UserConfig资料库，则创建；
  - seaf-cli download -l [library id] -s http://dev.openthos.org/ -u dev01@openthos.org -p 123 下载指定library
  - seaf-cli sync -l [library id] -d [library本地路径] -s http://dev.openthos.org/ -u dev01@openthos.org -tk [token] 同步library
  - seaf-cli desync -d [library本地路径] 解除同步
  - seaf-cli stop 停止
  
### 官方seaf-cli文档：
https://seacloud.cc/group/3/wiki/seafile-cli-manual.md
