# 构建 bootloader、内核、文件系统

> Linux 平台上有许多开源的嵌入式 linux 系统构建框架(框架的意思就是工 具)，这些框架极大的方便了开发者进行嵌入式系统的定制化构建，目前比较常 见的有 OpenWrt, Buildroot, Yocto,等等。其中 Buildroot 功能强大，使用 简单，而且采用了类似于 linux kernel 的配置和编译框架，所以受到广大嵌入 式开发人员的欢迎。

## 解压编译 bootloader

### bootloader 介绍

Bootloader 是在操作系统运行之前运行的一段代码，用于引导操作系统。 通常每个操作系统都有一组专属的引导加载程序。引导加载程序通常可以通过多种方式引导操作系统内核，还有各种命令用于调试或修改内核运行环境。

U-Boot 是一个开源的主引导加载程序，用于引导设备的操作系统内核，并含有多种命令以便调试系统。它适用于多种计算机体系结构，包括 68k，ARM， Blackfin，MicroBlaze，MIPS，Nios，SuperH，PPC，RISC-V 和 x86。

- U-boot 官网： https://www.denx.de/wiki/U-Boot 

- 源码下载页面： http://ftp.denx.de/pub/u-boot/ 

- NXP 官方 uboot 源码 Git 地址： https://source.codeaurora.org/external/imx/uboot-imx 

- ✅本开发使用的 U-boot 位于 Git 仓库，地址为： https://e.coding.net/weidongshan/imx-uboot2017.03.git

我们使用的版本针对板子进行过修改，u-boot 官网下载的源码不能直接使用。

### 编译 u-boot 镜像

不同的开发板对应不同的配置文件，配置文件位于 u-boot 源码的 “configs/”目录。对于 IMX6ULL Mini 版，u-boot 的编译过程如下(编译 uboot 前必须先配置好工具链等开发环境)：

``` bash
# 进入到 u-boot 源码目录
$ cd 100ask_imx6ull_mini-sdk/Uboot-2017.03/
# 清理编译文件
$ make distclean
# 进行配置
$ make mx6ull_14x14_evk_defconfig
# 编译
$ make
```

