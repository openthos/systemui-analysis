# 文件管理器说明书

## Note:
  - 文件管理器导航栏包括菜单栏和左侧导航栏。
  - 了解更多请查看下方（导航栏功能详情介绍）
  
## 文件管理器导航栏效果图
![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/filemanager.png)

## 常见任务
[浏览文件和文件夹](https://github.com/openthos/desktop-analysis/blob/master/instructions/%E6%B5%8F%E8%A7%88%E6%96%87%E4%BB%B6%E5%92%8C%E6%96%87%E4%BB%B6%E5%A4%B9.md)

[删除文件和文件夹](https://github.com/openthos/desktop-analysis/blob/master/instructions/%E5%88%A0%E9%99%A4%E6%96%87%E4%BB%B6%E5%92%8C%E6%96%87%E4%BB%B6%E5%A4%B9.md)

[复制或移动文件和文件夹](https://github.com/openthos/desktop-analysis/blob/master/instructions/%E5%A4%8D%E5%88%B6%E6%88%96%E7%A7%BB%E5%8A%A8%E6%96%87%E4%BB%B6%E5%92%8C%E6%96%87%E4%BB%B6%E5%A4%B9.md)

对文件和文件夹进行排序

搜索文件

重命名文件或文件夹

预览文件和文件夹

## 更多主题
使用其他应用程序打开文件

共享和传输文件

寻找丢失的文件

将文件写入 CD 或 DVD

文件属性

浏览服务器或网络共享中的文件

还原您删除的文件

文件管理器首选项

## 可移动驱动器和外部磁盘

安全删除外部设备

弹出或卸载 USB 闪存驱动器、CD、DVD 或其他设备。






## 菜单栏
  - 设置    (获得焦点，点击，弹出设置菜单)
    - 显示隐藏文件    (获得焦点，点击，显示系统内的隐藏文件)
    
  - 网格布局
     - 目前为网格布局，最大化窗口正常
     - 存在的bug：小窗口模式存在重叠现象
     - 目标：获得焦点，点击，以网格模式显示文件和文件夹
  
  - 列表布局
     - 暂未完成
     - 目前通过搜索栏搜索的文件为列表布局
     - 目标：获得焦点，点击，以列表模式显示文件和文件夹及其详情
  
  - 地址栏    (显示当前所在文件夹路径：输入有效路径，点击回车，可跳转到对应文件夹)
  
  - 搜索栏    (获得焦点，输入需要查找的文件名关键字，点击搜索按钮或回车键，可搜索到相应文件及文件夹)    
    - 进入个人空间、安卓系统、云盘后，支持搜索文件，在磁盘存储中会提示“当前路径下不能进行搜索。“

## 左侧导航栏
  - 收藏    (无点击事件)
    - 桌面    (获得焦点，点击，进入桌面文件夹)
    - 音乐    (获得焦点，点击，进入音乐文件夹)
    - 视频    (获得焦点，点击，进入视频文件夹)
    - 图片    (获得焦点，点击，进入图片文件夹)
    - 文档    (获得焦点，点击，进入文档文件夹)
    - 下载    (获得焦点，点击，进入下载文件夹)
    - 回收站    (获得焦点，点击，进入回收站文件夹)

  - 本机    (无点击事件)
    - 计算机    (获得焦点，点击，进入计算机首页)
    - USB    (移动设备识别后，增加USB导航，获得焦点，点击，进入可移动磁盘目录)
    - USB右侧符号    (获得焦点，点击，安全卸载移动设备)
  - 网络    (无点击事件)
    - 云服务    (获得焦点，点击，进入云服务界面)
    

## 计算机    (获得焦点，点击，进入计算机页面)
  - 本机存储
    - 个人空间    (获得焦点，双击，进入个人空间目录)
    - 安卓系统    (获得焦点，双击，进入安卓系统目录)
    - 磁盘存储    (获得焦点，双击，进入磁盘存储目录)
  - 移动设备
    - 可移动磁盘    (移动设备识别并弹出可移动磁盘界面后，双击，进入可移动磁盘目录)
  - 云服务
    - 云盘    (获取焦点，双击，进入云盘目录，云存储功能暂未实现)

## 文件目录
  - 文件夹    (获得焦点，双击，进入文件夹目录)
  - 文件    (获得焦点，双击，打开类型选择界面，选择相应的程序打开)
  - 复制    (选择文件或者文件夹，使用快捷键CTRL+C)
  - 粘贴    (进入要粘贴的目录,使用快捷键CTRL+V)
  - 剪切    (选择文件或者文件夹，使用快捷键CTRL+X)
  - 多选    (按住Ctrl键，选择多个文件或者Ctrl+A进行全选)
    - 可用快捷键进行复制、剪切
    - 单击右键可以进行复制、剪切

## 文件菜单   (单击鼠标右键，打开文件菜单)
![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/filemanager1.png)
  
  - 打开    (选择文件或者文件夹，单击鼠标右键，选择“打开“，可打开文件或者文件夹)
  - 打开方式    (选择文件，单击鼠标右键，选择“打开方式“，可选择文件的打开方式)
  - 压缩    (选择文件或者文件夹，单击鼠标右键，选择“压缩“，在弹出的对话框中选择压缩格式，点击确定(取消)来压缩(取消压缩)文件或者文件夹)
    - 压缩格式支持zip\tar
    ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/ziptar.png)
  - 解压缩    (选择压缩文件，单击鼠标右键，选择“解压缩“，可解压缩该文件至当前目录)
  - 复制    (选择文件或者文件夹，单击鼠标右键，选择"复制")
  - 粘贴    (进入要粘贴的目录，单击鼠标右键，选择"粘贴")
  - 重命名    (选择文件或者文件夹，单击鼠标右键，选择"重命名"，在弹出的对话框中输入新的文件(夹)的名称，点击确认(取消)来执行重命名(取消)的操作)
  - 删除    (选择文件或者文件夹，单击鼠标右键，选择"删除"，在弹出的对话框中选择确认(取消)来执行删除(取消)操作)
  - 剪切    (选择文件或者文件夹，单击鼠标右键，选择"剪切")
  - 发送    (选择文件，单击鼠标右键，选择“发送“，在弹出的对话框中选择分享方式)
    - 可选择某个方式分享仅此一次或者始终
    ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/Share.png)
    
    - 文件夹不能发送
    ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/Sendfile.png)
  
  - 排序    (任意处单击鼠标右键，选择“排序“，在弹出的对话框中选择排序的方式)
  ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/Sort.png)
  - 复制路径    (选择文件或者文件夹，单机鼠标右键，选择"复制路径"，可复制路径，在相应的地方使用CTRL+V可粘贴复制的路径)
  - 新建文件夹    (任意处单击鼠标右键，选择"新建文件夹"，在弹出的对话框中输入新建文件夹的名称，点击确认(取消)来创建文件夹(取消创建))
  - 创建文件    (任意处单击鼠标右键，选择"创建文件"，在弹出的对话框中输入新建文件的名称，点击确认(取消)来创建文件(取消创建))
    - 可选择创建文件的类型(.doc\.xls\.ppt\.txt)
    ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/creatfile.png)
  - 显示隐藏文件    (暂不支持)
    

## FileManager input

- **音乐**

- 1.音乐
  - 播放：可正常播放
  - 显示：可正常显示
  ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/music.png)

- 2.网易云音乐
  - 播放：可正常播放
  - 显示：可正常显示
  ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/cloudmusic.png)

- **视频**

- 1.vlc
  - 播放：可正常播放
  ![](https://github.com/openthos/systemui-analysis/blob/master/ImageView/vlc.png)

- **图片**

- 1.图库
  - 显示: 可正常显示
  - 多个图片快速浏览 :不支持
  ![](https://github.com/openthos/desktop-analysis/blob/master/imageView/gallery.png)
          
- 2.快图浏览
  - 显示 :可正常显示
  - 多个图片快速浏览 ：支持鼠标滚轮和键盘方向键快速浏览
  ![](https://github.com/openthos/desktop-analysis/blob/master/imageView/quickpick.png)
            
            
- **文档**

- 1.WPS
  - 页面： 内容显示正常
  - 文字： 显示正常
  - 编辑: 可使用中英文正常编辑
  - 保存: 可正常保存
  ![](https://github.com/openthos/desktop-analysis/blob/master/imageView/wps.png)
  ![](https://github.com/openthos/desktop-analysis/blob/master/imageView/wps_save.png)

- 2.Word
  - 页面： 内容显示正常
  - 文字： 显示正常
  - 编辑: 无法编辑
  ![](https://github.com/openthos/desktop-analysis/blob/master/imageView/Word.png)
- 3.文本编辑器
  - 页面： 内容显示正常
  - 文字： 显示字体偏小
  - 编辑: 可使用中英文正常编辑
  - 保存: 可正常保存      
  ![](https://github.com/openthos/desktop-analysis/blob/master/imageView/textEditor.png)
        
- **下载**
    
- 1.下载
  - 页面： 内容显示正常
  ![](https://github.com/openthos/desktop-analysis/blob/master/imageView/download.png)
  - 路径: 默认保存到个人空间的下载目录下
  ![](https://github.com/openthos/desktop-analysis/blob/master/imageView/download_dir.png)                          
