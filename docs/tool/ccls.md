---
comments: true
---

**ccls** 是一个支持 C、C++ 和 Objective-C 的语言服务器（Language Server Protocol，LSP）实现，专为高效的代码分析与开发设计。它支持代码补全、跳转、语法检查等功能，常用于编辑器如 VS Code、Vim 和 Emacs 的 C/C++ 开发环境。

本文主要介绍如何在 ubuntu 中使用源码编译安装 ccls。

在 ccls 的 [repo wiki]([Build · MaskRay/ccls Wiki](https://github.com/MaskRay/ccls/wiki/Build)) 中提到源码编译依赖的环境有：

- CMake 3.8 及以上
- C++ 编译器
  - Clang 5 及以上
  - GNU GCC 7.2 及以上
  - MSVC 2017及以上（ubuntu 上面不考虑）
- Clang + LLVM 的头文件以及相关库，版本需 >=7

## 安装 CMake

CMake 安装可到官网下载：[Download CMake](https://cmake.org/download/)，我选择的版本是 3.31：

```shell
wget https://github.com/Kitware/CMake/releases/download/v3.31.0/cmake-3.31.0.tar.gz
tar -zxvf cmake-3.31.0.tar.gz
./bootstrap
```

过程中可能缺少依赖，需要安装一些开发工具和第三方库：

```shell
sudo apt-get update
sudo apt-get install -y build-essential libssl-dev
```

然后编译 CMake

```shell
make
# 如果编译较慢，可以开启多核并行编译，后面的 4 是开启的核数
make -j4
```

最后编译安装

```shell
sudo make install
```

## 安装 Clang、相关开发依赖库

这一步可以直接安装：

```shell
sudo apt update
sudo apt install clang-18 libclang-18-dev
```

这将安装版本 18 的 clang，以及 clang 和 llvm 的库（头文件和 .a，.so 文件等等）。注意这里的命令仅限 Ubuntu，如果是其他系统，或者遇到了意外情况，可以去  [repo wiki]([Build · MaskRay/ccls Wiki](https://github.com/MaskRay/ccls/wiki/Build))  中找找有没有解决办法。

## 编译安装 ccls

```shell
# 下载源码
git clone --depth=1 --recursive https://github.com/MaskRay/ccls
cd ccls

# cmake 构建，注意需要找到自己系统里面的 llvm 相关文件路径
cmake -S. -BRelease -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_PREFIX_PATH=/usr/lib/llvm-18 \
    -DLLVM_INCLUDE_DIR=/usr/lib/llvm-18/include \
    -DLLVM_BUILD_INCLUDE_DIR=/usr/include/llvm-18/
# 编译安装
cd Release && sudo make install
```

然后就可以查看是否安装成功啦~

```shell
(base) kpole@kpole:~/ccls/Release$ whereis ccls
ccls: /usr/local/bin/ccls
```







