# 功能需求
  - 1.seaf-cli status获取动态显示到任务栏；
  - 2.Timer日常1min调用一次status，收到动态就1second调用一次status；
  - 3.Timer日常工作的同时FileObserver监听账号目录，只监听一层，监听到动态即停止监听，开始1second一次status;
