---
title: windows & office 激活
date: 2024-03-25 23:58:04
categories: 工具
tags:
  - 破解
---

## KMS 服务器激活

1. 以管理员身份运行 CMD
2. 在命令提示符中，依次输入以下命令： 

```powershell
slmgr /ipk W269N-WFGWX-YVC9B-4J6C9-T83GX
slmgr /skms kms.03k.org
slmgr /ato
```

以上命令首先会替换当前 Windows 的产品密钥，然后设置 KMS 服务器地址为 `kms.03k.org`，最后尝试激活 Windows。如果一切顺利，你应该能看到激活成功的弹窗。

<!-- more -->

## 脚本激活

以 [Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts) 为例，这是一个在 GitHub 上的开源项目，提供了一些用于激活 Windows & Office 的脚本。

1. 首先需要安装Office，如果没有安装 Office，可以访问 [O365ProPlus下载页](https://gravesoft.dev/download_windows_office/office_c2r_links/#chinese-simplified-zh-cn) 来下载。
2. 完成 Office 的安装后，打开 **PowerShell**，然后输入以下的命令：

```powershell
irm https://massgrave.dev/get | iex
```

这行命令将运行一个 PowerShell 脚本，该脚本会下载并运行激活脚本，从而完成 Office 的激活。
