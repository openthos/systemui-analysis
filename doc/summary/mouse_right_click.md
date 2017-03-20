## 鼠标右键的具体实现代码：
***

     public boolean onTouchEvent(MotionEvent e) {
    ┊   int button = e.getButtonState();
    ┊   int action = e.getAction();

    ┊   if(button == MotionEvent.BUTTON_SECONDARY && action == MotionEvent.ACTION_DOWN) {//  右键
    ┊   ┊   dismissDialog();
    ┊   ┊   mShowRBM = true;
    ┊   ┊   showDialog(getRbmView(), 0);
    ┊   ┊   return true;
    ┊   }          
    ┊   // Locked status to click
    ┊   if(action == MotionEvent.ACTION_DOWN) {//单击
    ┊   ┊   if(mActivity.mIsDocked) {
    ┊   ┊   ┊   if(!mActivity.mApkRun) {
    ┊   ┊   ┊   ┊   waitTimer();
    ┊   ┊   ┊   ┊   runApkByPkg();
    ┊   ┊   ┊   } else if(mActivity.mHiden) {
    ┊   ┊   ┊   ┊   resizeStack();
    ┊   ┊   ┊   }  
    ┊   ┊   } else if(mActivity.mHiden) {
    ┊   ┊   ┊   resizeStack();
    ┊   ┊   }      
    ┊   ┊   setFocusedStack();
    ┊   }          
    ┊   return super.onTouchEvent(e);
