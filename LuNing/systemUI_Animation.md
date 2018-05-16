# Animation动画
- 分类：Animations从总体上可以分为两大类<br \>
Tweened Animations：该类Animations提供了旋转、移动、伸展和淡出等效果。Alpha——淡入淡出，Scale——缩放效果，Rotate——旋转，Translate——移动效果。<br \>
Frame-by-frame Animations：这一类Animations可以创建一个Drawable序列，这些Drawable可以按照指定的时间间歇一个一个的显示。<br \>
- 使用TweenedAnimations的步骤：
- 代码中：<br \>
  1.创建一个AnimationSet对象（Animation子类）；<br \> 
```
    AnimationSet animationSet = new AnimationSet(true); 
```
　　2.增加需要创建相应的Animation对象；<br \> 
```
    AlphaAnimation alphaAnimation = new AlphaAnimation(1, 0); 
```
　　3.更加项目的需求，为Animation对象设置相应的数据；<br \>
```
    //设置动画执行的时间（单位：毫秒）  
    alphaAnimation.setDuration(1000);  
```
　　4.将Animatin对象添加到AnimationSet对象当中；<br \>
```
    animationSet.addAnimation(alphaAnimation);  
```
　　5.使用控件对象开始执行AnimationSet;<br \>
```
    imageView.startAnimation(animationSet); 
```
- xml文件中：<br \>
1.在res目录下新建目录anim；在下面新建xml文件，就做为Animation的布局文件;<br \>
2.首先加入set标签:<br \>
```
    <set android:shareInterpolator="false" xmlns:android="http://schemas.android.com/apk/res/android">
```
　　3.再在该标签当中加入rotate,alpha,scale,translate标签,例如状态栏时间模块的动画；
```
    <?xml version="1.0" encoding="utf-8"?>
    <set xmlns:android="http://schemas.android.com/apk/res/android" >
      <translate
        android:fromXDelta="100%"
        android:toXDelta="0"
        android:duration="500" />
      <alpha
        android:fromAlpha="0.0"
        android:toAlpha="1.0"
        android:duration="500" />
    </set>  
```
　　4.在代码中用AnimationUtils静态函数，加载动画xml文件;<br \>
```
    Animation animation = AnimationUtils.loadAnimation(HelloAnimationActivity.this, R.anim.animation_test);  
```
　　5.使用控件对象开始执行AnimationSet;<br \>
```
    imageView.startAnimation(animation); 
```
- Frame-by-FrameAmimations这一类可以创建一个Drawable序列：这些Drawable可以按照指定的时间间歇一个一个的显示;
- 使用方法：<br \>
1.定义资源文件xml：<br \>
```
    <animation-list xmlns:android="http://schemas.android.com/apk/res/android"  
      android:oneshot="true">  
      <item android:drawable="@drawable/rocket_thrust1" android:duration="200" />  
      <item android:drawable="@drawable/rocket_thrust2" android:duration="200" />  
      <item android:drawable="@drawable/rocket_thrust3" android:duration="200" />  
    </animation-list>  
```
　　2.在代码中使用资源文件：<br \>
```
    AnimationDrawable rocketAnimation;  
      
    public void onCreate(Bundle savedInstanceState) {  
      super.onCreate(savedInstanceState);  
      setContentView(R.layout.main);  
      
      ImageView rocketImage = (ImageView) findViewById(R.id.rocket_image);  
      rocketImage.setBackgroundResource(R.drawable.rocket_thrust);  
      rocketAnimation = (AnimationDrawable) rocketImage.getBackground();  
    }  
      
    public boolean onTouchEvent(MotionEvent event) {  
      if (event.getAction() == MotionEvent.ACTION_DOWN) {  
        rocketAnimation.start();  
        return true;  
      }  
      return super.onTouchEvent(event);  
    }  
```
