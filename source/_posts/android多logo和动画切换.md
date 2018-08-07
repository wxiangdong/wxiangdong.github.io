---
title: android多logo和动画切换
date: 2018-07-31 15:50:32
categories: "系统修改"
tags: 
     - MTK
     - 系统修改 
---

## 前言

基于mtk6580,添加多logo和开关机动画切换，

## 描述

目前android开机画面由三个部分（阶段）组成，第一部分在bootloader启动时显示（静态），第二部分在启动kernel时显示（静态），第三部分在系统启动时（bootanimation）显示（动画）。

<!-- more -->

### 添加资源

1. 在device/tangxun/tx6580_weg_m/ProjectConfig.mk,找到BOOT_LOGO=这项,记住这项内容(如hd720,),在vendor/mediatek/proprietary/bootable/bootloader/lk/dev/logo/目录下找到BOOT_LOGO=对应的文件夹把你的图片放进去，图片我是这样命名的hd720_kernel_i7.bmp.(如果你只是替换的话更换hd720_kernel.bmp和hd720_uboot.bmp这两张图片即可，新图片的名字需与旧图片一致)
2. 在vendor/mediatek/proprietary/bootable/bootloader/lk/dev/logo/rules.mk下修改RESOURCE_OBJ_LIST列表，如图：
{% asset_img add_logo_image_1.png %}
最后两项就是我添加的
3. 同目录下update文件中添加
{% asset_img add_logo_image_2.png %}
4. 

### 添加标识区分不同logo

思路：首先我们添加的标识，不能被轻易清除，包括恢复出厂设置情况下。所以我选择在protect_f分区下创建空文件的方式，在show——logo的时候判断相应文件是否存在。
1. 选择一种要展示的logo和动画，在protect_f分区下创建.dat后缀的文件,删除其他类型动画在protect_f分区下的相应文件
```
private void createOrDeleteFile(String str){
        String sDir = "/protect_f";
        File fDir = new File(sDir);
        if (fDir.exists()){
            try {
                Runtime.getRuntime().exec("chmod 777"+sDir);
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        File mFile = new File(sDir,File_moto_logo);
        if (mFile.exists()){
            mFile.delete();
        }

        mFile = new File(sDir,File_samsun_logo);
        if (mFile.exists()){
            mFile.delete();
        }

        mFile = new File(sDir,"sysBoot_logo_null.dat");
        if (mFile.exists()){
            mFile.delete();
        }

        if (str != null){
            mFile = new File(sDir,str);
            if (!mFile.exists()){
                try {
                    mFile.createNewFile();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

```
2. 在vendor/mediatek/proprietary/external/libshowlogo/charging_animation.cpp文件中，添加logo切换
```
const char LOGO_ON5_ANI[] = "/protect_f/sysBoot_logo_moto.dat";
const char LOGO_I7_ANI[] = "/protect_f/sysBoot_logo_samsun.dat";

/*
 * Show kernel logo when phone boot up
 *
 */
void show_kernel_logo()
{
    SLOGD("[libshowlogo: %s %d]show kernel logo, index = 38 \n",__FUNCTION__,__LINE__);
    if (error_flag == 0)
    {
        if(open(LOGO_ON5_ANI,O_RDONLY) >= 0)
        {
            anim_show_logo(kernel_logo_position+1);
            property_set("ani_type","custom");
            property_set("animation_num","On5_Ani");
        }else if (open(LOGO_I7_ANI,O_RDONLY) >= 0)
        {
            anim_show_logo(kernel_logo_position+2);
            property_set("ani_type","custom");
            property_set("animation_num","I7_Ani");
        }else{
            anim_show_logo(kernel_logo_position);
            property_set("ani_type","android");
            property_set("animtion_num","android");
        }
    }
}
```
3. framworks/base/cmds/bootanimation/BootAnimation.cpp文件中，在void BootAnimation::initBootanimationZip()方法中添加切换动画
```
#endif
    char anitype[PROPERTY_VALUE_MAX];
    char aninum[PROPERTY_VALUE_MAX];
    property_get("ani_type",anitype,"");
    property_get("animation_num",aninum,"");
    if (strcmp("custom",anitype) == 0)
    {
        if (strcmp("On5_Ani", aninum)==0) {
            if (access("/system/media/bootanimation_custom.zip", R_OK) == 0) {
                if ((zipFile = ZipFileRO::open("/system/media/bootanimation_custom.zip")) != NULL) {
                    mZip = zipFile;
                }
            }
        }else if (strcmp("I7_Ani", aninum)==0){
            if (access("/system/media/bootanimation_s6.zip", R_OK) == 0) {
                if ((zipFile = ZipFileRO::open("/system/media/bootanimation_s6.zip")) != NULL) {
                    mZip = zipFile;
                }
            }
        }
    }

    if (zipFile == NULL) {
```

到这里功能基本就可以实现了。

