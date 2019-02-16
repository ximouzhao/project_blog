---
title: Android编译Github源代码构建出错
date: 2018-11-30 18:42:39
tags:
---
# 一、NDK问题
今天下下载了https://github.com/pengliangAndroid/VirtualLocation的源码导入到AndroidStudio之后构建工具报错分别如下
 1.报错显示 mips64el-linux-android-strip 找不到
2.[could not resolve all dependencies for configuration ':app:debugAPK']
3.ndkBuild Could not resolve all dependencies for configuration

#### 尝试了一天之后终于找到了解决办法：
从 [https://developer.android.google.cn/ndk/downloads/older_releases](https://developer.android.google.cn/ndk/downloads/older_releases) 下载16b版本的ndk到本地, 并解压说后替换本地的NDK之后该错误解决。参考https://blog.csdn.net/bylaven/article/details/80331345

# 二、butterknife构建
报错如下：Unable to find method 'com.android.build.gradle.api.BaseVariant.getOutputs()Ljava/util/List;'.

解决方法：将butterknife的版本从8.8.1降至8.4.0

# 三、butterknife简介
ButterKnife是一个专注于Android系统的View注入框架,以前总是要写很多findViewById来找到View对象，有了ButterKnife可以很轻松的省去这些步骤。是大神JakeWharton的力作，目前使用很广。最重要的一点，使用ButterKnife对性能基本没有损失，因为ButterKnife用到的注解并不是在运行时反射的，而是在编译的时候生成新的class。项目集成起来也是特别方便，使用起来也是特别简单。


参考：https://www.jianshu.com/p/3678aafdabc7
