---
layout:     post
title:      QEMU + GDB Installation for Daniel Page's Lab
subtitle:   "ARM开发环境配置指南"
date:       2020-02-19
author:     "Elvis"
catalog: true
comments: true
guid: urn:uuid:bd69ccfc-e840-4bc4-99ec-54cefac7f749
tags:
    - Linux
	- ARM
	- GDB
---



### Install the ARM Package

1. Download <a href="https://releases.linaro.org/components/toolchain/binaries/5.1-2015.08/arm-eabi/gcc-linaro-5.1-2015.08-x86_64_arm-eabi.tar.xz">**ARM GCC Compiler**</a> to your Linux Downloads directory.

2. Extract the file with `tar -zxvf ×××.tar.gz`.

   ```shell
   ~ cd Downloads/
   tar xvzf gcc-linaro-5.1-2015.08-x86_64_arm-eabi.tar.xz 
   ```

3. Move the directory to `/opt/software` which is the target directory of labsheets' `MAKEFILE`.

   ```shell
   ~ cd
   sudo cp -r Downloads/gcc-linaro-5.1-2015.08-x86_64_arm-eabi /opt/software/
   ```

   

### Install QEMU emulator

> QEMU is a generic and open source machine emulator and virtualizer.

[QEMU](https://www.qemu.org/download/) is packaged by most Linux distributions, for Debian distributions (e.g. Ubuntu), we use `apt-get`.

```shell
apt-get install qemu
```

