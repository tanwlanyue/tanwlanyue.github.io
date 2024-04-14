---
title: OpenHarmony开发准备
date: 2024-03-29 23:20:53
categories: 
tags:
  - ohos
---
# 环境准备

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

# 拉取代码
repo init -u git@gitee.com:openharmony/manifest.git -b master --no-repo-verify  
repo sync -c  
repo forall -c 'git lfs pull'

build/prebuilts_download.sh --no-check-certificatie -skip-ssl  

./build.sh --no-prebuilt-sdk --product-name=rk3568 --ccache \
-T foundation/multimedia/camera_framework/frameworks/native/camera:camera_framework \
camera_napi camera_service -j32 --fast-rebuild

# 同步代码
git remote add openharmony https://gitee.com/openharmony/multimedia_camera_framework.git
git fetch openharmony
git rebase openharmony/master
```

<!-- more -->
加快本地编译的一些参数，适当选择添加以下的编译参数可以加快编译的过程。

- --ccache : ccache会缓存c/c++编译的编译输出，下一次在编译输入不变的情况下，直接复用缓存的产物
- --fast-rebuild : 编译流程主要分为preloader->loader->gn->ninja这四个过程，在本地没有修改gn和产品配置相关文件的前提下，添加--fast-rebuild 会直接从 ninja 编译开始
- --build-target参数 : 该参数用于指定编译模块，可以通过相关仓下BUILD.gn中关注group、ohos_shared_library、ohos_executable等关键字找模块的名字

# vscode 插件 clangd 安装

项目执行prebuild会下载clang,不需要自己apt install clang

![](https://raw.githubusercontent.com/tanwlanyue/images/master/202404150129013.png)

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

# 别名脚本
initEnv.sh
```sh
echo "alias prebuild='build/prebuilts_download.sh --no-check-certificatie -skip-ssl'" >> ~/.bashrc
echo "alias buildcamera='./build.sh --no-prebuilt-sdk --product-name=rk3568 --ccache -T foundation/multimedia/camera_framework/frameworks/native/camera:camera_framework camera_napi camera_service -j32 --fast-rebuild'" >> ~/.bashrc
source ~/.bashrc
echo "------init environment over------"
```

```sh
wget https://gitee.com/username/projectname/raw/branch/initEnv.sh
chmod 777 initEnv.sh
source initEnv.sh
```