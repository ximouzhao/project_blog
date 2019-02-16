---
title: Android 无障碍和receiver的两个注意点
date: 2018-11-16 15:40:08
tags: Android Receiver
---

## 1.AccessibilityService无效果 接收不到onAccessibilityEvent事件

在配置好AccessibilityService后，而且系统“辅助设置”已打开配置过的AccessibilityService，刚开始能用，但莫名出现onAccessibilityEvent事件接收不到的情况，原因只有一条：

程序出现了ANR，崩溃后AccessibilityServices就失效了，重启手机即可

---------------------

原文：https://blog.csdn.net/bunny1024/article/details/78139740

## 2.广播监听android.net.conn.CONNECTIVITY_CHANGE 网络变化无法收到广播

根据https://developer.android.com/topic/performance/background-optimization的提示，api level 24以后通过manifest注册的[CONNECTIVITY_ACTION](https://developer.android.com/reference/android/net/ConnectivityManager.html#CONNECTIVITY_ACTION) 将不再发送广播，解决方法是通过actity的oncreate方法注册
```java
MyReceiver receiver=new MyReceiver();

IntentFilter filter=new IntentFilter();

filter.addAction(ConnectivityManager.CONNECTIVITY_ACTION);

this.registerReceiver(receiver,filter);
```


### google文档如下

Background processes can be memory- and battery-intensive. For example, an implicit broadcast may start many background processes that have registered to listen for it, even if those processes may not do much work. This can have a substantial impact on both device performance and user experience.

To alleviate this issue, Android 7.0 (API level 24) applies the following restrictions:

Apps targeting Android 7.0 (API level 24) and higher do not receive [CONNECTIVITY_ACTION](https://developer.android.com/reference/android/net/ConnectivityManager.html#CONNECTIVITY_ACTION) broadcasts if they declare their broadcast receiver in the manifest. Apps will still receive [CONNECTIVITY_ACTION](https://developer.android.com/reference/android/net/ConnectivityManager.html#CONNECTIVITY_ACTION) broadcasts if they register their [BroadcastReceiver](https://developer.android.com/reference/android/content/BroadcastReceiver.html) with [Context.registerReceiver()](https://developer.android.com/reference/android/content/Context.html#registerReceiver(android.content.BroadcastReceiver,%20android.content.IntentFilter)) and that context is still valid.

Apps cannot send or receive [ACTION_NEW_PICTURE](https://developer.android.com/reference/android/hardware/Camera.html#ACTION_NEW_PICTURE) or [ACTION_NEW_VIDEO](https://developer.android.com/reference/android/hardware/Camera.html#ACTION_NEW_VIDEO) broadcasts. This optimization affects all apps, not only those targeting Android 7.0 (API level 24).

If your app uses any of these intents, you should remove dependencies on them as soon as possible so that you can properly target devices running Android 7.0 or higher. The Android framework provides several solutions to mitigate the need for these implicit broadcasts. For example, [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) and the new [WorkManager](https://developer.android.com/arch/work) provide robust mechanisms to schedule network operations when specified conditions, such as a connection to an unmetered network, are met. You can now also use [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) to react to changes to content providers. [JobInfo](https://developer.android.com/reference/android/app/job/JobInfo.html) objects encapsulate the parameters that [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) uses to schedule your job. When the conditions of the job are met, the system executes this job on your app's [JobService](https://developer.android.com/reference/android/app/job/JobService.html).

In this document, we will learn how to use alternative methods, such as [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html), to adapt your app to these new restrictions.
·
![TIM截图20181116154910.png](https://upload-images.jianshu.io/upload_images/14992936-4d73e00e5925c702.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
