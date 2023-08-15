# QEMULauncher

代码来自我的腐竹：
````
@echo off
title 腐竹专用QEMU简易启动器
color 2f
goto main

:main
echo 你好，%username%
echo 欢迎使用 QEMU简易启动器
echo 您的版本是：定制版
echo =========================================
echo 1. 创建磁盘
echo 2. 启动虚拟机
echo 3. 安装虚拟机
echo 4. 给磁盘加空间（目前仅支持qcow2,qcow后缀的磁盘）
echo 5. 退出
echo 6. VNC方式启动｜安装系统
echo 7. VNC方式启动｜安装旧版系统
echo =========================================
set/p xuhao=请输入序号：
if "%xuhao%"=="1" goto createdisk
if "%xuhao%"=="2" goto startup
if "%xuhao%"=="3" goto install
if "%xuhao%"=="4" goto resize
if "%xuhao%"=="5" exit
if "%xuhao%"=="6" goto vnc
if "%xuhao%"=="7" goto oldinstall


:createdisk
set/p diskname=请输入磁盘名字：
set/p disktype=请输入磁盘类型（qcow2,qcow,raw,vmdk）：
set/p diskmg=请输入磁盘大小（M,G）：
echo 正在创建磁盘
D:
cd qemu
qemu-img create -f %disktype% %diskname%.%disktype% %diskmg%
set/p disknum=磁盘创建完成，是否移动到桌面？（1.是 2.否）：
if "%disknum%"=="1" move %diskname%.%disktype% C:\Users\%username%\desktop\
cls
goto main
if "%disknum%"=="2" goto main

:startup
set/p shyn=是否同时创建启动脚本？（1.是 2.否）
if "%shyn%"=="1" goto startup1
if "%shyn%"=="2" goto startup2


:startup1
set/p shname=请输入脚本名字：
set/p smp1=请输入核心数：
set/p m1=请输入内存大小：
set/p hda1=请拖入磁盘文件：
echo 正在创建脚本
echo D:\qemu\qemu-system-x86_64 -machine q35 -cpu EPYC-v4 -boot menu=on -smp %smp1% -m %m1% -usb -device usb-kbd -device usb-tablet -nic user,model=virtio -hda %hda1% -boot menu=on >%shname%.bat
echo 正在通过脚本启动QEMU
.\%shname%.bat
cls
goto main


:startup2
set/p smp2=请输入核心数：
set/p m2=请输入内存大小：
set/p hda2=请拖入磁盘文件：
echo 正在启动QEMU
D:\qemu\qemu-system-x86_64 -M q35 -cpu EPYC-v4 -boot menu=on -smp %smp2% -m %m2% -usb -device usb-kbd -device usb-tablet -nic user,model=virtio -hda %hda2% -boot menu=on
cls
goto main


:install
set/p smp3=请输入核心数：
set/p m3=请输入内存大小：
set/p hda3=请拖入磁盘文件：
set/p cdrom1=请拖入ISO文件：
echo 正在启动QEMU安装客户机操作系统
D:\qemu\qemu-system-x86_64 -M q35 -cpu EPYC-v4 -boot menu=on -smp %smp3% -m %m3% -usb -device usb-kbd -device usb-tablet -nic user,model=virtio -hda %hda3% -cdrom %cdrom1% -boot menu=on
cls
goto main


:resize
set/p hda4=请拖入磁盘文件（仅支持qcow,qcow2）：
set/p addg=请输入增加多少G（写单位,M或者G）：
echo 正在增加磁盘大小...
qemu-img resize %hda4% +%addg%
echo 增加完成
pause
cls
goto main


:vnc
set/p smp4=请输入核心数：
set/p m4=请输入内存大小：
set/p hda4=请拖入磁盘文件：
set/p cdrom2=请拖入ISO文件：
set/p vncp1=请输入VNC端口：
echo 正在启动QEMU安装客户机操作系统
D:\qemu\qemu-system-x86_64 -M q35 -boot menu=on -smp %smp4% -m %m4% -usb -device usb-kbd -device usb-tablet -nic user,model=virtio -hda %hda4% -cdrom %cdrom2% -boot menu=on -vnc :%vncp1% -vga vmware
cls
goto main

:oldinstall
set/p smp5=请输入核心数：
set/p m5=请输入内存大小：
set/p hda5=请拖入磁盘文件：
set/p cdrom3=请拖入ISO文件：
set/p vncp2=请输入VNC端口：
echo 正在启动QEMU安装客户机操作系统
D:\qemu\qemu-system-x86_64 -boot menu=on -smp %smp5% -m %m5% -usb -device usb-kbd -device usb-tablet -nic user,model=e1000 -hda %hda5% -cdrom %cdrom3% -boot menu=on -vnc :%vncp2% -vga vmware
cls
goto main
````
