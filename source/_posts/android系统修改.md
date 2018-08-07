---
title: android系统修改
date: 2018-07-27 10:28:50
categories: "系统修改"
tags:
	 - MTK
     - 系统修改 
---

## 前言

以下都是基于mtk平台，系统相关修改点，在此记录。

<!-- more -->

1. 修改wifi信号强度
framwork/base/wifi/java/android/net/wifi/WifiManager.java 修改culculateSignalLevel方法中的rssi
2. 3G信号强度修改
vendor/mediatek/proprietary/frameworks/base/packages/FwkPlugin/src/com/mediatek/op/telephony/DefaultServiceStateExt.java 
3. dex2oat优化模式
frameworks/native/cmds/installd/commands.cpp
```
 strcpy(dex2oat_compiler_filter_arg, "--compiler-filter=interpret-only");
 have_dex2oat_compiler_filter_flag = true;
```
修改模式为interpret-only，烧录开机时间加快，安装播放器之类的大型应用速度加快
4. 设置中打印项修改
packages/apps/Settings/src/com/android/settings/print/PrintSettingsFragment.java
5. 蓝牙wifi名称修改
device/mediatek/common/custom.conf 	
6. imei相关
vendor/mediatek/proprietary/packages/apps/EngineerMode/src/com/mediatek/engineermode/SsWriteImeiActivity.java
vendor/mediatek/proprietary/packages/apps/EngineerMode/src/com/mediatek/engineermode/GPRS.java
7. 机型名称
build/tools/buildinfo.sh 	
修改ro.product.model=“要修改的型号”
8. 设置中内核信息和版本号
packages/apps/Settings/src/com/android/settings/DeviceInfoSettings.java
9. BUILD_VERNO修改
MTK_BUILD_VERNO = alps-mp-m0.mp1-V2.115
10. 去掉开机引导
packages/apps/Launcher3/src/com/android/launcher3/Launcher.java
注释launcherClings.showLongPressCling(true)
11.隐藏设置中的某一项
packages/apps/Settings/src/com/android/settings/SettingsActivity.java
```
    Bundle metaData = activityInfo.metaData;
-   if ((metaData == null) || !metaData.containsKey(EXTRA_CATEGORY_KEY)) {
+   /*if ((metaData == null) || !metaData.containsKey(EXTRA_CATEGORY_KEY)) {*/    // add by wxd for hide google in settings
+   if ((metaData == null) || !metaData.containsKey(EXTRA_CATEGORY_KEY) || activityInfo.packageName.equals("com.google.android.gms")) {
```
12. 修改默认横屏
framwork/base/services/core/java/com/android/server/policy/PhoneWindowManager.java
修改rotationForOrientationLw方法，直接返回Surface.ROTATION_90
13. 修改hotseat横屏时在底部

