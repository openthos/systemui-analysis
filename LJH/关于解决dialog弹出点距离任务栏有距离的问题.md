### 关于解决dialog弹出点距离任务栏有距离的问题

 - 我们在右键点击固定在任务栏上的应用时，会弹出dialog进行操作，但是系统字体调大以后，这个dialog距离任务栏之间会有一个距离产生，这是之前没有做适配的原因造成的。
 - 之前弹dialog的主要步骤是
  - 当弹出的点是view的时候（packages/SystemUI/src/com/android/systemui/statusbar/policy/ActivityKeyView.java）
  ***
      lp.x = location[0] - dpx / 2 + iconSize - iconSize / DIALOG_OFFSET_PART;
      lp.y = location[1] - dpy / 2 - view.getMeasuredHeight() + DIALOG_OFFSET_DIMENSION - padding;
  ***  
  - 当弹出的点是鼠标点击位置的时候（packages/SystemUI/src/com/android/systemui/statusbar/phone/PhoneStatusBar.java）
  ***
      lps.x = x - dpx / 2 + iconSize +  iconSize;
      lps.y = dpy / 2 - iconSize - iconSize / DIALOG_OFFSET_PART_THREE - DIMENSION_OFFSET_LOCATION;
  ***
   - 这里面的DIALOG_OFFSET_PART，DIALOG_OFFSET_DIMENSION，padding，DIALOG_OFFSET_PART_THREE，DIMENSION_OFFSET_LOCATION都是凑出来的数据，没有做出适配，当字体变化后就出现了问题
   
 - 解决方案：
   - 首先需要确认dialog弹出点的位置坐标，要确定弹出点的位置坐标就需要一个基准点（0，0）的坐标
     - dialogWindow.setGravity(Gravity.CENTER);
   - 为什么设置Gravity.CENTER的呢？设置Gravity.LEFT，Gravity.TOP，Gravity.BOTTOM等不行吗。这其中就涉及了一个问题，例如当设置属性中Gravity.LEFT的时候，（0，0）这个点的x轴并不是以左侧的边界点开始的，但这个距离是多少无法确定，为了避免这个内容，就设置（0，0）为屏幕的中心点了
   - 弹出点的坐标确定
     - 当弹出的点是view的时候（packages/SystemUI/src/com/android/systemui/statusbar/policy/ActivityKeyView.java
     *** 
           lp.x = location[0] + itemWidth / 2 - mPhoneBar.getScreenWidth() / 2;
           lp.y = mPhoneBar.getScreenHeight() / 2 - statusBarSize - view.getMeasuredHeight() / 2;
           其中location[0]是这个view的左上角相对于整个屏幕的坐标，（0，0）点是整个屏幕的左上角，itemWidth是这个view的宽度，mPhoneBar.getScreenWidth()是整个屏幕的宽度，mPhoneBar.getScreenHeight()整个屏幕的高度，statusBarSize是任务栏高度，view.getMeasuredHeight()是弹出的dialog的高度
     ***
     - 当弹出的点是鼠标点击位置的时候（packages/SystemUI/src/com/android/systemui/statusbar/phone/PhoneStatusBar.java）
     ***
          lp.x = x - getScreenWidth() / 2;
          lp.y = getScreenHeight() / 2 - statusBarSize - view.getMeasuredHeight() / 2;
          x是鼠标点击相对于整个屏幕的绝对坐标，getScreenWidth()是屏幕的宽度，getScreenHeight（）是屏幕的高度，statusBarSize是任务栏的高度，view.getMeasuredHeight()是弹出的dialog的高度
     ***
     
   
 
