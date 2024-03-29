---
title: OpenHarmony开发准备
date: 2024-03-29 23:20:53
categories:
---

<!-- more -->

拉取代码

```sh
sudo sed -i "s@http://.*archive.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list
sudo apt-get update
sudo apt install python3-pip
sudo ln -s /usr/bin/python3 /usr/bin/python
sudo apt-get install git-lfs

sudo curl https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 -o /usr/local/bin/repo
sudo chmod a+x /usr/local/bin/repo
pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests

repo init -u git@gitee.com:openharmony/manifest.git -b master --no-repo-verify  
repo sync -c  
repo forall -c 'git lfs pull'

build/prebuilts_download.sh --no-check-certificatie -skip-ssl  

./build.sh --no-prebuilt-sdk --product-name=rk3568 --ccache -T foundation/multimedia/camera_framework/frameworks/native/camera:camera_framework camera_napi camera_service -j32

自己仓同步代码
git remote add openharmony https://gitee.com/openharmony/multimedia_camera_framework.git
git fetch openharmony
git rebase openharmony/master
```


vscode 插件 clangd 安装

项目执行prebuild会下载clang,不需要自己apt install clang

<img src="https://raw.githubusercontent.com/tanwlanyue/image/master/202403292322696.png" alt="image">

编译命令添加`--gn-flags='--export-compile-commands'`

```
./build.sh --no-prebuilt-sdk --product-name=rk3568 --gn-flags='--export-compile-commands'
```

因为build参数`--product-name =rk3568`, `compile_commands.json` 生成在out/rk3568目录下

编辑工程目录下的 .vscode/settings.json

```json
{
    "clangd.path": "${workspaceFolder}/prebuilts/clang/ohos/linux-x86_64/llvm/bin/clangd",
    "clangd.arguments": [
        "--compile-commands-dir=${workspaceFolder}/out/rk3568/",
        "--query-driver=${workspaceFolder}/prebuilts/clang/ohos/linux-x86_64/llvm/bin/clang++",
    ],
}
```

vscode 底部显示 indexing:xxx/xxxx 索引进度即设置成功
