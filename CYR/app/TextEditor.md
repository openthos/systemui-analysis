## TextEditor文本编辑器
***

### 前言
  - 该富文本编辑器: 内核使用的是ace(https://githup.com/ajaxorg/ace) , 所以编译该项目需要先编辑ace内核。
  - 前提: 先安装 node node.js npm
  - 方法: 
    - cd tools/assets/ace/
    - node ./Makefile.dryice.js
  - 常出现的问题:
    - 会出现一些问题: 比如Modules　thow error . 可以查看(https://github.com/nodejs/help/issues/663) 进行解决。
    
### 整体结构
  - 服务器代码位置: origin  git://192.168.0.185/TextEditor.git
  - 项目工程(app)
    - ui包
      - 编辑器的主要界面的搭建。
  - file_explore
    - 文件浏览module用于打开/另存为/保存时，文件存储位置的浏览.
  - common
  - styles
    - 这两个module主要是一些辅助。
    
***

### 核心代码
  - assets资源文件夹
    - ace文件夹
      - ace内核
      - 这里都是js的文件
    - css
      - 文本编辑区的样式
  - 这部分核心编辑区的开发需要web的知识。

### 功能需求
  - 这个主要还是原项目的内容
  - 原项目: https://github.com/jecelyin/920-text-editor-v2
  
### 项目逻辑代码分析
  - 
