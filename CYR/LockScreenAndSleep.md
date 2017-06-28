### 关于锁屏的代码流程

***
  - I/Matthew -- > DEBUG( 2516): KeyguardViewMediator -- > showLocked(Bundle options)
  - I/Matthew --> DEBUG( 2516): StatusBarKeyguardViewManager -- > showBounceOrKeyguard()
  - I/Matthew -- > DEBUG( 2516): PhoneStatusBar --- > showKeyguard()
  - I/Matthew -- >  DEBUG( 2680): PhoneStatusBar   showStatusBarViewPoerSleep()
  
  - I/Matthew -- > DEBUG( 2516): PowerManager -- > goToSleep(long time)
  - I/Matthew -- > DEBUG:( 2390): KeyguardServiceDelegate -- > onScreenTurnedOff(int why)
  - I/Matthew -- > DEBUG( 2390): KeyguardServiceWrapper --> onScreenTurnedOff(int reason)
  - I/Matthew -- > DEBUG( 2516): KeyguardService --> onScreenTurnedOff
  - I/Matthew -- > DEBUG( 2714): StatusBarKeyguardViewManager  -- > onScreenTurnedOff()
  - I/Matthew  -- > DEBUG( 2714): PhoneStatusBar -- > onScreeenTurnedOff()
  - I/Matthew --> DEBUG( 2516): StatusBarKeyguardViewManager -- > showBounceOrKeyguard()
  - I/Matthew -- > DEBUG( 2516): PhoneStatusBar --- > showKeyguard()
  
  ***
### 休眠的代码流程
  - I/Matthew -- >  DEBUG( 2680): PhoneStatusBar   showStatusBarViewPoerSleep()
  - I/Matthew -- > DEBUG( 2500): PowerManager -- > goToSleep(long time)
  - I/Matthew -- > DEBUG:( 2390): KeyguardServiceDelegate -- > onScreenTurnedOff(int why)
  - I/Matthew -- > DEBUG( 2390): KeyguardServiceWrapper --> onScreenTurnedOff(int reason)
  - I/Matthew -- > DEBUG( 2500): KeyguardService --> onScreenTurnedOff
  - I/Matthew -- > DEBUG( 2500): KeyguardViewMediator -- > showLocked(Bundle options)
  - I/Matthew -- > DEBUG( 2714): StatusBarKeyguardViewManager  -- > onScreenTurnedOff()
  - I/Matthew  -- > DEBUG( 2714): PhoneStatusBar -- > onScreeenTurnedOff()
  - I/Matthew --> DEBUG( 2500): StatusBarKeyguardViewManager -- > showBounceOrKeyguard()
  - I/Matthew -- > DEBUG( 2500): PhoneStatusBar --- > showKeyguard()

***
#### 解决问题方案:
  - 从代码流程可以看出, 都是从PowerManager的 goToSleep进入休眠, 故: showStatusBarViewPowerSleep()方法中代码需要控制;
  - 1. 通过 广播进行 对 flag控制, 然后进行控制流程. goToSleep  只能控制这里, 之后的onScreenTurnOff 还需要方法, (现象: 锁屏后, 停顿一会后休眠)
  - 2. 改变流程(具体实施, 待测) 
  - 3. 休眠的主要方法在: PowerManagerService.java 中的: reallyGoToSleepNoUpdateLocked(SystemClock.uptimeMillis(), Process.SYSTEM_UID);
  - 4. 由于锁屏后的休眠和自动休眠是走的一个逻辑:PowerManager.GO_TO_SLEEP_REASON_TIMEOUT 故修改了锁屏后的休眠, 自动休眠也存在影响;
  - 5. 目前已经区别了休眠和锁屏的区别, 要实现锁屏不进入休眠, 还 不形象自动休眠, 需要更多时间调研, 目前暂停在这里.
