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

./build.sh --no-prebuilt-sdk --product-name=rk3568 --ccache -T foundation/multimedia/camera_framework/frameworks/native/camera:camera_framework camera_napi camera_service -j32 --fast-rebuild

# 同步代码
git remote add openharmony https://gitee.com/openharmony/multimedia_camera_framework.git
git fetch openharmony
git rebase openharmony/master
```

<!-- more -->
加快本地编译的一些参数，适当选择添加以下的编译参数可以加快编译的过程。
- **添加–ccache参数**:
    - 原理：ccache会缓存c/c++编译的编译输出，下一次在编译输入不变的情况下，直接复用缓存的产物。
    - 安装：
        - 快速安装：执行sudo apt-get install ccache命令。
        - [官网下载](https://ccache.dev/download.html)，下载二进制文件，把ccache所在路径配置到环境变量。
    - 使用：执行./build.sh --product-name 产品名 --ccache命令。
- **添加–fast-rebuild参数**
    - 原理：编译流程主要分为：preloader->loader->gn->ninja这四个过程，在本地没有修改gn和产品配置相关文件的前提下，添加–fast-rebuild会让你直接从ninja编译开始。
    - 使用：执行./build.sh --product-name 产品名 --fast-rebuild命令。
- **添加enable_notice_collection=false参数**
    - 原理：省略掉收集开源软件模块的license的过程。
    - 使用：执行./build.sh --product-name 产品名 --gn-args --enable_notice_collection=false --ccache命令。
- **添加–build-target参数**
    - 该参数用于指定编译模块，如何找模块的名字：
        - 相关仓下BUILD.gn中关注group、ohos_shared_library、ohos_executable等关键字。
        - ./build.sh --product-name 产品名 --build-target 模块名 --build-only-gn生成build.ninja，然后去该文件中查找相关模块名。
    - 使用：执行./build.sh --product-name 产品名 --build-target ark_js_host_linux_tools_packages命令。

# vscode 插件 clangd 安装

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