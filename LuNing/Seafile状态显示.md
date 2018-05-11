# 功能需求
  - 1.seaf-cli status获取动态显示到通知栏；
  - 2.Timer日常1min查看一次status，检测到动态后Timer周期改为就1秒，获取状态信息发送到通知栏；
  - 3.Timer日常工作的同时FileObserver监听DATA和UserConfig，只监听一层，监听到动态即停止监听，Timer周期改为1秒，发送通知;
  
## 实现
- 1.seaf-cli status获取状态信息：
```
    Runtime.getRuntime().exec(new String[]{"su", "-c", command});
```
- 2.Timer监听：
```
    // 开启Timer
    StatusTask mStatusTask = new StatusTask();
    StatusTimer mStatusTimer = new Timer();
    mStatusTimer.schedule(mStatusTask, delay, period);
    
    private class StatusTask extends TimerTask {
        @Override
        public void run() {
        // 调用命令行获取status信息，并检测
        // 如发现状态改变即发送或消除Notification，修改Timer周期，或
        showNotification(notice);
        restartTimer(TIMER_SHORT);
        }
    ｝
    
    private void restartTimer(long period) {
        if (period == TIMER_LONG) {
            mDATAObserver.startWatching();
            mUserConfigObserver.startWatching();
        } else {
            mDATAObserver.stopWatching();
            mUserConfigObserver.stopWatching();
        }
        mStatusTask.cancel();
        mStatusTimer.cancel();
        mStatusTask = new StatusTask();
        mStatusTimer = new Timer();
        mStatusTimer.schedule(mStatusTask, period, period);
    }
```
