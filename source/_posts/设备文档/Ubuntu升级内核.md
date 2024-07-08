---
title: Ubuntu升级内核
categories: 项目文档
author: 简得辉
tags:
  - 项目文档
keywords: Ubuntu升级内核
abbrlink: 20240603173105
data: 2024/06/03 17:31
---


## 内核下载解压
```bash
wget https://gh.con.sh/https://github.com/jadehh/pythonTools/releases/download\
/JadeV2.2.4/linux-6.9.3.tar.xz
tar -xvf linux-6.9.3.tar.xz && sudo  mv linux-6.9.3 /usr/src/ && rm linux-6.9.3.tar.xz

```


##  Ubuntu18 环境安装
```bash
sudo apt-get -y install libncurses5-dev libssl-dev pkg-config bison flex libelf-dev
```

## Ubuntu20 环境安装

```bash
sudo apt-get -y install libncurses5-dev libssl-dev pkg-config bison flex libelf-dev zstd dwarves
```

## 配置内核

```bash
cd /usr/src/linux-6.9.3
sudo make mrproper && sudo make clean && sudo make menuconfig
```

> 直接选择exit，然后按回车。

## 编译内核

### Ubuntu 20 编译

```
sudo wget https://gh.con.sh/https://github.com/jadehh/pythonTools/releases/download\
/JadeV2.2.4/config 
sudo mv config .config
sudo make -j16
```

### Ubuntu18 编译

```
sudo make -j16
```

## 安装内核

```bash
sudo make -j16 INSTALL_MOD_STRIP=1 modules_install
sudo make install 
```


## 更新内核

```bash
sudo update-grub
```
