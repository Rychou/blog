---
title: Linux操作系统下连接闪讯的方法（支持有线与无线）
date: 2018-10-05 14:23:29
tags: [Linux, 闪讯]
---

# 一、前言

用过电信闪讯的同学都知道，闪讯没有开发 Linux 的客户端程序，所以这让很多玩 Linux 操作系统同时又是闪讯用户的同学很头疼，今天我就来介绍一下如何在 Linux 下连接闪讯网络，并且支持有线连接和无线连接。

<!-- more -->

本教材基于这个`Github`仓库[shanxun_pppoe_linux_desktop](https://github.com/Elrori/shanxun_pppoe_linux_desktop)

# 二、使用教程

### 1. 克隆此仓库到本地

[shanxun_pppoe_linux_desktop](https://github.com/Elrori/shanxun_pppoe_linux_desktop)

### 2. 安装 ppp 依赖

```shell
sudo apt-get install ppp
```

### 3. 修改或确认 makefile 文件的 TODO 项

makefile 中的 TODO 项都有相关的注释，不同的系统可能有不同的配置，应先确认。

### 4. 安装和编译

在仓库目录下执行：

```shell
sudo chmod a+x INSTALL
sudo ./INSTALL zhejiang_xiaoyuan_sxplugin
#(其他地区请更换,地区列表在makefile内)
```

### 5. 连接闪讯网络

由于闪讯网络有两种方式连接，有线和无线，方法略有不同。

首先在命令行执行`ifconfig`命令，记录下有线网卡和无线网卡名称，有线网卡名称是`e`开头的，无线网卡名称是`w`开头的，不同设备的名称不一样。

**5.1 有线连接**

直接使用以下命令连接有线：

```shell
sudo connect_sx <username> <password>
```

参数分别是闪讯用户名和密码

```shell
# 其后直接使用这个命令就可以了
sudo connect_sx

# 更改密码
sudo connect_sx <password>

# 断开连接
sudo connect_sx stop
```

若连接不上，则尝试以下命令后再次尝试连接。

```shell
sudo del default route
sudo add default route <有线网卡名称>
sudo connect_sx <username> <password>
```

**5.2 无线连接**

使用以下命令连接

```shell
pppd noauth nodetach defaultroute usepeerdns maxfail 1 user <your username> password <your password> mtu 1492 mru 1492 plugin rp-pppoe.so nic-<网卡名> plugin zhejiang_xiaoyuan_sxplugin.so
```

需要配置的参数有用户名和密码，以及你的无线网卡名，此方法也可以用来连接有线，在网卡名填入对应的有线网卡名即可。

# 三、连接脚本

针对以上连接方式，为了便于连接，我写了以下脚本，仅供参考。

```shell
#!/bin/bash
# 是否连接有线
ETH_STATE=`cat /sys/class/net/enp4s0/operstate`
# 是否连接无线
WLAN_STATE=`cat /sys/class/net/wlp1s0/operstate`
# 连接的无线网络名称 其中wlp1s0是无线网卡名
WLAN_NAME=`iwconfig wlp1s0 | grep ESSID | awk -F\" '{print $2}'`
# 账号密码
username=12345678911@DZKD.XY
password=123456

# 有线连接
if [ "up" == $ETH_STATE ]
then
	connect_sx
fi

# 无线连接
if [ "up" == $WLAN_STATE ]
then
	# 我所在环境能连闪讯的有三个无线WIFI
	if [ "ChinaNet-shanxun" == $WLAN_NAME ]||[ "ChinaNet-tRmt" == $WLAN_NAME ]||[ "ChinaNet-tRmt-5G" == $WLAN_NAME ]
	then
		pppd noauth nodetach defaultroute usepeerdns maxfail 1 user $username password $password mtu 1492 mru 1492 plugin rp-pppoe.so nic-wlp1s0 plugin zhejiang_xiaoyuan_sxplugin.so
	else
		echo "is not shanxun"
	fi
fi
```

在后台运行脚本：

```shell
sudo nohup ./shanxun.sh &
```
