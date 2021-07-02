# Compile OP-TEE, U-Boot and Linux for RK3229


### Prequisites

- A host computer running a Ubuntu 21.04
- An armhf toolchain
- GNU make
- git


### Prepare the build environment

```
$ sudo apt install make python3-pip python3-pyelftools gcc-arm-linux-gnueabihf \
  g++-arm-linux-gnueabihf bison flex device-tree-compiler libssl-dev -y
$ pip3 install pycryptodome
$ mkdir build
$ build() { log=$1; shift 1; (date; echo; time make $@) 2>&1 | tee $log; }
$ export BUILD=`readlink -f build`
$ export ARCH=arm
$ export CROSS_COMPILE=arm-linux-gnueabihf-
```


### Build OP-TEE

```
$ cd $BUILD
$ git clone https://github.com/anbuhckr/rk3229.git
$ git clone https://github.com/OP-TEE/optee_os.git
$ cd optee_os
$ git checkout -b 3.7.0/rk3229 3.7.0
$ build build-optee.log CFG_TEE_BENCHMARK=n CFG_TEE_CORE_LOG_LEVEL=3 DEBUG=1 PLATFORM=rockchip-rk322x -j2
```


### Build U-Boot

```
$ cd build
$ git clone git://git.denx.de/u-boot.git
$ cd u-boot
$ git checkout -b v2020.01/rk3229 v2020.01
$ cp ../optee_os/out/arm-plat-rockchip/core/tee-pager.bin tee.bin
```

*Apply the following patch only if your device does not have an eMMC (such as the MXQ 4K and MXQ Pro 4K) or if it has an eMMC that is not detected by U-Boot in its present state.*

```
$ patch -Np1 < ../rk3229/patch/u-boot/sdmmc-dm-pre-reloc.patch
```

```
$ patch -Np1 < ../rk3229/patch/u-boot/0001-rockchip-sdram-fix-DRAM-bank-declaration-around-OP-T.patch
$ patch -Np0 < ../rk3229/patch/u-boot/xms6-rk3229_defconfig.patch
$ make xms6-rk3229_defconfig
$ sed -i 's/YYLTYPE yylloc/extern YYLTYPE yylloc/g' scripts/dtc/dtc-lexer.lex.c
$ build build-uboot.log -j2 
```


### Build Linux

```
$ cd $BUILD
$ git clone --branch v5.8 --depth 1 git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
$ cd linux
$ cp ../rk3229/config/linux.config .config
$ make oldconfig
$ build build-zImage.log -j2 zImage
$ build build-dtbs.log -j2 dtbs
$ build build-modules.log -j2 modules
```
