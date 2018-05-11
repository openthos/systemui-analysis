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
        // Timer不能重复调用来修改周期，只能停止之前的Timer和TimerTask，并重新创建，重新启动
        // 如不重新创建，Timer会报状态已经启动的异常，TimerTask会报正在运行的异常
        mStatusTask.cancel();
        mStatusTimer.cancel();
        mStatusTask = new StatusTask();
        mStatusTimer = new Timer();
        // schedule(TimerTask t, long delay)两参方法不周期
        mStatusTimer.schedule(mStatusTask, delay, period);
    }
```
- 3.发送Notification
```
    NotificationManager mNotificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
    NotificationCompat.Builder mBuilder = new NotificationCompat.Builder(this);
    // 设置标题
    mBuilder.setContentTitle(getString(R.string.seafile_status_title));
    // 设置小图标，显示在通知左侧，必须设置，否侧通知发不出去
    mBuilder.setSmallIcon(R.mipmap.ic_launcher);
    mBuilder.setDefaults(NotificationCompat.DEFAULT_ALL);
    // true则点击通知即消失
    mBuilder.setAutoCancel(false);
    
    private void showNotification(String notice) {
        // 默认设置内容为Builder.setContentText，文字不会换行，"\n"也不生效
        // 使用BigTextStyle可设置多行文字，支持换行
        mStyle = new NotificationCompat.BigTextStyle();
        mStyle.bigText(notice);
        mBuilder.setStyle(mStyle);
        mNotification = mBuilder.build();
        // flags通知为正在运行中
        mNotification.flags |= Notification.FLAG_ONGOING_EVENT;
        // when右上角时间，不设置的默认时间为通知第一次发出时的时间，即使之后再次notify时间也不变，
        // 且会导致bug此通知与其它通知位置不断变换，设置when后，此通知会始终在最顶
        mNotification.when = System.currentTimeMillis();
        // 发送与刷新通知均使用notify
        // 0为Notification的唯一标识，可任意设置，删除时使用根据标识cancel(0)
        mNotificationManager.notify(0, mNotification);
    }
```
- 4.
