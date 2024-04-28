---
title: windows & office 激活
date: 2024-03-25 23:58:04
categories: 工具
tags:
  - open
---

1. 以管理员身份运行CMD

```powershell
slmgr /ipk W269N-WFGWX-YVC9B-4J6C9-T83GX
slmgr /skms kms.03k.org
slmgr /ato
```

<!-- more -->

2. [Microsoft-Activation-Scripts](https://github.com/massgravel/Microsoft-Activation-Scripts)

[O365ProPlus下载页](https://gravesoft.dev/download_windows_office/office_c2r_links/#chinese-simplified-zh-cn)

```powershell
# PowerShell
irm https://massgrave.dev/get | iex
```

