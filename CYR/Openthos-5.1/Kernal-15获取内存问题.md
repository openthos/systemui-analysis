## Kernal-15获取内存问题

***
#### 测试patch
```
 core/java/android/app/ActivityManager.java                          | 3 +++
 core/jni/android_os_Debug.cpp                                       | 6 ++++--
 core/jni/android_util_Process.cpp                                   | 1 +
 .../core/java/com/android/server/am/ActivityManagerService.java     | 4 ++++
 4 files changed, 12 insertions(+), 2 deletions(-)

diff --git a/core/java/android/app/ActivityManager.java b/core/java/android/app/ActivityManager.java
index e155157..5801505 100644
--- a/core/java/android/app/ActivityManager.java
+++ b/core/java/android/app/ActivityManager.java
@@ -30,6 +30,7 @@ import android.os.ParcelFileDescriptor;
 import com.android.internal.app.ProcessStats;
 import com.android.internal.os.TransferPipe;
 import com.android.internal.util.FastPrintWriter;
+import android.util.Log;
 
 import android.content.ComponentName;
 import android.content.Context;
@@ -1705,6 +1706,7 @@ public class ActivityManager {
      * manage its memory.
      */
     public void getMemoryInfo(MemoryInfo outInfo) {
+        Log.i("Smaster_AM_getMemoryInfo", "outInfo" + outInfo);
         try {
             ActivityManagerNative.getDefault().getMemoryInfo(outInfo);
         } catch (RemoteException e) {
@@ -2265,6 +2267,7 @@ public class ActivityManager {
      * requested pid.
      */
     public Debug.MemoryInfo[] getProcessMemoryInfo(int[] pids) {
+        Log.i("Smaster_AM_getProcessMemoryInfo", "pids" + pids);
         try {
             return ActivityManagerNative.getDefault().getProcessMemoryInfo(pids);
         } catch (RemoteException e) {
diff --git a/core/jni/android_os_Debug.cpp b/core/jni/android_os_Debug.cpp
index d9e64c7..cc62772 100644
--- a/core/jni/android_os_Debug.cpp
+++ b/core/jni/android_os_Debug.cpp
@@ -21,6 +21,7 @@
 #include "utils/misc.h"
 #include "cutils/debugger.h"
 #include <memtrack/memtrack.h>
+#include <utils/Log.h>
 
 #include <cutils/log.h>
 #include <fcntl.h>
@@ -528,6 +529,7 @@ static jlong android_os_Debug_getPssPid(JNIEnv *env, jobject clazz, jint pid, jl
                         c++;
                     }
                     pss += atoi(c);
+                    ALOGI("Smaster_android_os_Debug_getPssPid_pss ================================ %ld kB", pss);
                 } else if (strncmp(line, "Private_Clean:", 14) == 0
                         || strncmp(line, "Private_Dirty:", 14) == 0) {
                     char* c = line + 14;
@@ -535,10 +537,10 @@ static jlong android_os_Debug_getPssPid(JNIEnv *env, jobject clazz, jint pid, jl
                         c++;
                     }
                     uss += atoi(c);
+                    ALOGI("Smaster_android_os_Debug_getPssPid_uss================================= %ld MB", uss);
                 }
             }
         }
-
         fclose(fp);
     }
 
@@ -561,7 +563,7 @@ static jlong android_os_Debug_getPssPid(JNIEnv *env, jobject clazz, jint pid, jl
             env->ReleaseLongArrayElements(outMemtrack, outMemtrackArray, 0);
         }
     }
-
+    ALOGI("Smaster_android_os_Debug_getPssPid_return= %ld MB", pss / 1024);
     return pss;
 }
 
diff --git a/core/jni/android_util_Process.cpp b/core/jni/android_util_Process.cpp
index aaa680f..2c5554e 100644
--- a/core/jni/android_util_Process.cpp
+++ b/core/jni/android_util_Process.cpp
@@ -913,6 +913,7 @@ static jlong android_os_Process_getPss(JNIEnv* env, jobject clazz, jint pid)
     fclose(file);
 
     // Return the Pss value in bytes, not kilobytes
+    ALOGI("Smaster_android_os_Process_getPss pss = %ld", pss * 1024);
     return pss * 1024;
 }
 
diff --git a/services/core/java/com/android/server/am/ActivityManagerService.java b/services/core/java/com/android/server/am/ActivityManagerService.java
index 5760fef..8a8e3e9 100755
--- a/services/core/java/com/android/server/am/ActivityManagerService.java
+++ b/services/core/java/com/android/server/am/ActivityManagerService.java
@@ -1877,6 +1877,7 @@ public final class ActivityManagerService extends ActivityManagerNative
                                 }
                             }
                             nativeTotalPss += Debug.getPss(st.pid, null, null);
+                            Log.i("Smaster_nativeTotalPss", "==" + nativeTotalPss);
                         }
                     }
                     memInfo.readMemInfo();
@@ -5625,6 +5626,7 @@ public final class ActivityManagerService extends ActivityManagerNative
                     }
                 }
             }
+            Log.i("Smaster_AMS_getProcessMemoryInfo", "infos" + infos[i]);
         }
         return infos;
     }
@@ -5644,6 +5646,7 @@ public final class ActivityManagerService extends ActivityManagerNative
             }
             long[] tmpUss = new long[1];
             pss[i] = Debug.getPss(pids[i], tmpUss, null);
+            Log.i("Smaster_AMS_getProcessPss", "pss" + pss[i]);
             if (proc != null) {
                 synchronized (this) {
                     if (proc.thread != null && proc.setAdj == oomAdj) {
@@ -14756,6 +14759,7 @@ public final class ActivityManagerService extends ActivityManagerNative
                 ProcessCpuTracker.Stats st = mProcessCpuTracker.getStats(i);
                 if (st.vsize > 0) {
                     long pss = Debug.getPss(st.pid, null, memtrackTmp);
+                    Log.i("Smaster_AM_reportMemUsage", "pss" + pss);
                     if (pss > 0) {
                         if (infoMap.indexOfKey(st.pid) < 0) {
                             ProcessMemInfo mi = new ProcessMemInfo(st.name, st.pid,
-- 
2.7.4
```

### 对比kernal-15 和 kernal-9
  - kernal-15:AMS获取到部分数据；
  - android_os_Debug.cpp 的android_os_Debug_getPssPid方法获取也只是部分数据；
  - android_os_Debug 对应--> Debug.java  对应方法是getPss()
  - 测试kernal-9获取的数据正常
  
  
### 重点分析代码
  - 位置: core/jni/android_os_Debug.cpp
  - 函数： android_os_Debug_getPssPid
```
 515     sprintf(tmp, "/proc/%d/smaps", pid);
 516     fp = fopen(tmp, "r");
 517     ALOGI("Smaster_android_os_Debug_getPssPid_pss pid =========== %d ", pid);
 518     if (fp != 0) {
 519         while (true) {
 520             if (fgets(line, 1024, fp) == NULL) {
 521                 break;
 522             }
 523 
 524             if (line[0] == 'P') {
 525                 if (strncmp(line, "Pss:", 4) == 0) {
 526                     char* c = line + 4;
 527                     ALOGI("Smaster_android_os_Debug_getPssPid_pss line ================ %s ", line);
 528                     while (*c != 0 && (*c < '0' || *c > '9')) {
 529                         c++;
 530                     }
 531                     ALOGI("Smaster_android_os_Debug_getPssPid_pss ccccccc======== %s ", c);
 532                     pss += atoi(c); 
 533                     ALOGI("Smaster_android_os_Debug_getPssPid_pss (2)================================ %ld kB", pss);
 534                 } else if (strncmp(line, "Private_Clean:", 14) == 0
 535                         || strncmp(line, "Private_Dirty:", 14) == 0) {
 536                     char* c = line + 14;
 537                     while (*c != 0 && (*c < '0' || *c > '9')) {
 538                         c++;
 539                     }
 540                     uss += atoi(c);
 541                     ALOGI("Smaster_android_os_Debug_getPssPid_uss================================= %ld kB", uss);
 542                 }
 543             }
 544         }
 545         ALOGI("Smaster_android_os_Debug_getPssPid_pss (3) &&&&&&&&&&& %ld kB", pss);
 546         fclose(fp);
 547     }
         ALOGI("Smaster_android_os_Debug_getPssPid_return= %ld KB", pss );
```
  
### 针对kernal-15获取到部分数据原因分析

```
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_pss =pid  3459
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_return= 0 MB === total 0
I/Smaster_AMS_getProcessPss( 2903): pss0
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_pss =pid  3645
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_return= 0 MB === total 0
I/Smaster_AMS_getProcessPss( 2903): pss0
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_pss =pid  3558
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_return= 0 MB === total 0
I/Smaster_AMS_getProcessPss( 2903): pss0
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_pss =pid  6440
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_pss &&&&&&&&&&& 0 kB
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_pss line ============= Pss:                   0 kB
I/android.os.Debug( 2903):
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_pss ccccc====== 0 kB
I/android.os.Debug( 2903):
I/android.os.Debug( 2903): Smaster_android_os_Debug_getPssPid_pss ================================ 0 kB

```
**log中可以看出 pid获取到， 但是没有进入文件读取**

#### 原因猜测
  - 1. 文件打开失败，权限问题。
  - 2. 资源在被其他进程访问，无法读取。
  

