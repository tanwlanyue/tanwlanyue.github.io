---
title: 安装WSL
date: 2024-03-26 01:22:30
categories: 
tags:
  - ohos
---

控制面板→程序→启用或关闭Windows功能→{ Hyper-V, 适用于Linux的Windows子系统 }

<img src="https://raw.githubusercontent.com/tanwlanyue/images/master/202404150137787.png" style="zoom:67%;" />

设置→开发者选项→开发人员模式

以管理员身份运行cmd

```shell
# WSL2需要开启win10的虚拟机平台特性
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
# 开启Hyper-V，会导致Vmware软件用不了!!!
dism.exe /Online /Enable-Feature /All /FeatureName:Microsoft-Hyper-V
```

<!-- more -->

[更新wsl2内核 | wsl_update_x64.msi](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

```shell
# 切换到wsl2
wsl --set-default-version 2
```

[下载ubuntu20.04 | CanonicalGroupLimited.UbuntuonWindows_2004.2021.825.0.AppxBundle](https://aka.ms/wslubuntu2004)

```shell
# 进入wsl2环境
bash
# 设置root密码
sudo passwd
# 创建/删除账号
useradd --create-home -s /bin/bash z00856280
passwd z00856280
usermod -aG sudo z00856280
userdel -r z00856280
```

[下载LxRunOffline-v3.5.0-msvc](https://github.com/DDoSolitary/LxRunOffline/releases/download/v3.5.0/LxRunOffline-v3.5.0-msvc.zip)

```shell
# 关闭运行中的WSL2（否则将无法移动安装目录）
wsl.exe  --shutdown
# 移动指定的WSL2系统到目标目录
.\LxRunOffline.exe m -n Ubuntu -d D:\WSL2\Ubuntu
# 查看路径进行确认
lxrunoffline di -n Ubuntu
```

给ext4.vhdx权限

<img src="https://raw.githubusercontent.com/tanwlanyue/images/master/202404150138366.png" style="zoom:50%;" />

添加一个网络位置 `\\wsl$\Ubuntu`

传文件可以使用网络位置直接拖拽，也可以通过挂载的`/mnt` 目录进行cp操作

```shell
# wsl 卸载子系统
wsl --unregister Ubuntu
```
