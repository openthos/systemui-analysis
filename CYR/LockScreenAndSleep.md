### 关于锁屏的代码流程

***
  - I/Matthew -- > DEBUG( 2516): KeyguardViewMediator -- > showLocked(Bundle options)
  - I/Matthew --> DEBUG( 2516): StatusBarKeyguardViewManager -- > showBounceOrKeyguard()
  - I/Matthew -- > DEBUG( 2516): PhoneStatusBar --- > showKeyguard()

  - I/Matthew -- > DEBUG( 2516): PowerManager -- > goToSleep(long time)
  - I/Matthew -- > DEBUG:( 2390): KeyguardServiceDelegate -- > onScreenTurnedOff(int why)
  - I/Matthew -- > DEBUG( 2390): KeyguardServiceWrapper --> onScreenTurnedOff(int reason)
  - I/Matthew -- > DEBUG( 2516): KeyguardService --> onScreenTurnedOff
  - I/Matthew --> DEBUG( 2516): StatusBarKeyguardViewManager -- > showBounceOrKeyguard()
  - I/Matthew -- > DEBUG( 2516): PhoneStatusBar --- > showKeyguard()
  
  ***
### 休眠的代码流程
  - I/Matthew -- > DEBUG( 2500): PowerManager -- > goToSleep(long time)
  - I/Matthew -- > DEBUG:( 2390): KeyguardServiceDelegate -- > onScreenTurnedOff(int why)
  - I/Matthew -- > DEBUG( 2390): KeyguardServiceWrapper --> onScreenTurnedOff(int reason)
  - I/Matthew -- > DEBUG( 2500): KeyguardService --> onScreenTurnedOff
  - I/Matthew -- > DEBUG( 2500): KeyguardViewMediator -- > showLocked(Bundle options)
  - I/Matthew --> DEBUG( 2500): StatusBarKeyguardViewManager -- > showBounceOrKeyguard()
  - I/Matthew -- > DEBUG( 2500): PhoneStatusBar --- > showKeyguard()
