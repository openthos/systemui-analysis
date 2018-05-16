# 状态栏自定义电量功能实现总结
## 1.剩余电量
- 在BroadcastReceiver的onReceive()事件，接收到的Intent.ACTION_BATTERY_CHANGED，包括下面的信息：    
“status”（int类型）…状态，定义值是BatteryManager.BATTERY_STATUS_XXX   
“health”（int类型）…健康，定义值是BatteryManager.BATTERY_HEALTH_XXX    
“present”（boolean类型   
“level”（int类型）…电池剩余容量  
“scale”（int类型）…电池最大值。通常为100   
“icon-small”（int类型）…图标ID    
“plugged”（int类型）…连接的电源插座，定义值是BatteryManager.BATTERY_PLUGGED_XXX    
“voltage”（int类型）…mV    
“temperature”（int类型）…温度，0.1度单位。例如 表示197的时候，意思为19.7度    
“technology”（String类型）…电池类型，例如，Li-ion等等    
计算剩余电量百分比代码:
``` 
    int level = (int)(100f * intent.getIntExtra(BatteryManager.EXTRA_LEVEL, 0) / 
                 intent.getIntExtra(BatteryManager.EXTRA_SCALE, 100));
``` 
## 2.剩余电量可使用时间
- BatteryStatsHelper主要作用是用来计算所有应用和服务的用电量信息，使用BatteryStatsHelper接口的时候，必须在Activity或者Fragment初始化的时候，先new一个实例，再调用BatteryStatsHelper的create()来初始化它，同时在activity或者Fragment销毁的时候调用destroy()方法来销毁它。create()方法有两个，本次用的是Bundle参数的；
``` 
    public void create(Bundle icicle) {  
        if (icicle != null) {  
            mStats = sStatsXfer;  //静态的BatteryStats
            mBatteryBroadcast = sBatteryBroadcastXfer;  //静态的Intent 
        }  
        mBatteryInfo = IBatteryStats.Stub.asInterface(  
                ServiceManager.getService(BatteryStats.SERVICE_NAME));  
        mPowerProfile = new PowerProfile(mContext);  
    }
``` 
- Create()方法中主要做了两件事：  
(1) 获取了一个BatteryStatsService服务的对象。用于和BatteryStatsService进行通信。  
(2) 创建了一个PowerProfile文件对象。  
我们知道Android系统中关于电量计算公式：耗电量=模块使用时间*　单位时间内的耗电量，模块使用时间,已经在ＢatteryStatsService中统计过了，单位时间的耗电量在PowerProfile文件定义好了，每个手机的power_profile文件都不一样，需要厂商自己定义，只需要根据各个厂商定义的这个文件来计算相关的耗电量即可。
- BatteryStatsHelper的refreshStats()方法用来刷新计算的用电量信息，是BatteryStatsHelper类的关键接口，电量计算也是在该方法在中进行处理的，代码如下：
``` 
     public void refreshStats(int statsType, SparseArray<userhandle> asUsers, long rawRealtimeUs,
            long rawUptimeUs) {
        //初始化基本的状态
        mMaxPower = 0;
        mMaxRealPower = 0;
        mComputedPower = 0;
        mTotalPower = 0;
        //重置相关的列表
        ……
        //初始化相关的电量计算工具，各种计算工具都继承自PowerCalculator类，用于计算不同模块的耗电量
        if (mCpuPowerCalculator == null) {
            mCpuPowerCalculator = new CpuPowerCalculator(mPowerProfile);
        }
        mCpuPowerCalculator.reset();
 
        ……
 
        if (mFlashlightPowerCalculator == null) {
            mFlashlightPowerCalculator = new FlashlightPowerCalculator(mPowerProfile);
        }
        mFlashlightPowerCalculator.reset();
  
        //初始化相关的信息
        ……
        //获取电池剩余时间
        mBatteryTimeRemaining = mStats.computeBatteryTimeRemaining(rawRealtimeUs);
        //获取充电剩余时间
        mChargeTimeRemaining = mStats.computeChargeTimeRemaining(rawRealtimeUs);
        ……
        //计算各个应用的耗电量
        processAppUsage(asUsers);
 
        ……
 
        //计算其他杂项的耗电量
        processMiscUsage();
  
        Collections.sort(mUsageList);
 
        
        //计算耗电量最大的应用，并计算所有的应用的总耗电量
        if (!mUsageList.isEmpty()) {
            mMaxRealPower = mMaxPower = mUsageList.get(0).totalPowerMah;
            final int usageListCount = mUsageList.size();
            for (int i = 0; i < usageListCount; i++) {
                mComputedPower += mUsageList.get(i).totalPowerMah;
            }
        }
 
        mTotalPower = mComputedPower;
        //统计的总耗电量和实际总耗电量之间的差距
        if (mStats.getLowDischargeAmountSinceCharge() > 1) {
            //电池耗电量大于统计的总耗电量
            if (mMinDrainedPower > mComputedPower) {
                double amount = mMinDrainedPower - mComputedPower;
                mTotalPower = mMinDrainedPower;
                BatterySipper bs = new BatterySipper(DrainType.UNACCOUNTED, null, amount);
 
                // Insert the BatterySipper in its sorted position.
                int index = Collections.binarySearch(mUsageList, bs);
                if (index < 0) {
                    index = -(index + 1);
                }
                mUsageList.add(index, bs);
                mMaxPower = Math.max(mMaxPower, amount);
            //电池耗电量小于统计的总耗电量
            } else if (mMaxDrainedPower < mComputedPower) {
                double amount = mComputedPower - mMaxDrainedPower;
 
                // Insert the BatterySipper in its sorted position.
                BatterySipper bs = new BatterySipper(DrainType.OVERCOUNTED, null, amount);
                int index = Collections.binarySearch(mUsageList, bs);
                if (index < 0) {
                    index = -(index + 1);
                }
                mUsageList.add(index, bs);
                mMaxPower = Math.max(mMaxPower, amount);
            }
        }
    }
``` 
- 上面方法中可以看出mStats.computeBatteryTimeRemaining(rawRealtimeUs)方法可以直接获取电量剩余使用时间;mStats是BatteryStats(抽象类)，调用BatteryStatsHelper的getStats()方法获取;得到的剩余使用时间是毫秒，经过格式化得到天/小时/分钟/秒，代码如下：
``` 
    mBatteryStats =  mBatteryStatsHelper.getStats();
    long chargeTimeRemaining = mBatteryStats.computeChargeTimeRemaining(elapsedRealtime);
    String chargeString = Formatter.formatShortElapsedTime(getContext(),chargeTimeRemaining / 1000);
    strBatteryRemaining = mContext.getResources().getString(R.string.charge_remaining_string,chargeString);
    mBatteryRemaining.setText(strBatteryRemaining);
``` 
- 充满电需要的时间的获取类似电量剩余可使用时间，调用如下方法：
``` 
    mChargeTimeRemaining = mStats.computeChargeTimeRemaining(rawRealtimeUs);
```     
- 在获取系统电量剩余时间或充电剩余时间时，由于计算过程较复杂，需要考虑到硬件、软件等，等待的时间较长，此时以上两个方法返回的值均为-1，所以此时为界面考虑设置了默认值（3小时30分钟），在获取到真实值后立即会刷新。

## 3.根据电池状态更换图标
- 目前只提供了三种状态的电池图标，分别为充电中、高电量、低电量（<10%）。具体实现是通过判断电量剩余百分比和电池状态，为控件设置对应的图片，代码如下：
``` 
    if (intent.getAction().equals(Intent.ACTION_BATTERY_CHANGED)) {
        int level = (int)(100f * intent.getIntExtra(BatteryManager.EXTRA_LEVEL, 0)
                     / intent.getIntExtra(BatteryManager.EXTRA_SCALE, 100));
        int status = intent.getIntExtra(BatteryManager.EXTRA_STATUS, -1);
        boolean isCharging = status == BatteryManager.BATTERY_STATUS_CHARGING ||
                             status == BatteryManager.BATTERY_STATUS_FULL;
        if (isCharging) {
            mBatteryButton.setImageDrawable(mContext.getDrawable(
                                            R.drawable.statusbar_battery));
        } else {
            if (level >= 10) {
                mBatteryButton.setImageDrawable(mContext.getDrawable(
                                                R.drawable.statusbar_battery_high));
            } else {
                mBatteryButton.setImageDrawable(mContext.getDrawable(
                                                    R.drawable.statusbar_battery_low));
            }
        }
    }
``` 
