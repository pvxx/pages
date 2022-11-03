### 1.下载第三方rom，下列为已知的



​    https://download.pixelexperience.org/raphael 

​     https://t.me/redmik20proupdates

### 2.解压platform-tool  

plateform-tool 是fastboot和adb工具，用来刷机

### 3 进入twrp的rec

```
fastboot boot recovery.img  #用twrp的rec
```

### 4.通过rec格式化所有分区，

```
1、先通过twrp高级格式化，但是注意不要把Vendor分区给格了
2、由于Twrp分区分的不对，然后重启进入小米本身的rec，重新格式化，小米rec会正确分区
```

### 5. 拷贝rom到手机，安装rom

很简单，不再赘述

### 6.下载最新Magisk

  https://github.com/topjohnwu/Magisk/releases 

### 7. ROOT 方式一

```
1、[Magisk-v23.0.apk](https://github.com/topjohnwu/Magisk/releases/download/v23.0/Magisk-v23.0.apk) 更改为 Magisk-v23.0.zip ；其它版本方法一样，把apk后缀更改而已
2、再次使用twrp 临时启动，刷入Magisk-v23.0.zip
3、进入手机，打开Magisk，安装更新，安装到rec，直接安装，即可获取root权限
```

### 8、ROOT 方式二

```
1、找到刷机包的boot.img
2、使用Magisk修补boot.img
3、进入fastboot把修补后的boot.img重新刷入: fastboot flash boot boot.img
```

