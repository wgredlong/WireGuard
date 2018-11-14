# 用 WireGuard 科学上网

![https://www.wireguard.com/img/wireguard.svg](https://www.wireguard.com/img/wireguard.svg)

WireGuard 的立意是创建「 快速、现代、安全的 VPN 隧道 」。

通过 WireGuard 组建起来的 VPN ，借助其中一台可以科学上网的电脑，能使 VPN 内所有其它电脑都具有科学上网的能力。

本文简要介绍 WireGuard 的组网要点及步骤，其它四篇介绍相关的操作方法。

> 1. [用 Linode 主机搭建 WireGuard 网络](1.用%20Linode%20主机搭建%20WireGuard%20网络.md) ，实例介绍 Linode 服务器与本地 Ubuntu 电脑组建 WireGuard 网络的步骤；
> 2. [用 wg-quick 调用 wg0.conf 管理 WireGuard](2.用%20wg-quick%20调用%20wg0.conf%20管理%20WireGuard.md) ，如何配置 wg0.conf 的各项参数；
> 3. [用 wg 在命令行运行 WireGuard](3.用%20wg%20在命令行运行%20WireGuard.md) ，通过命令行使用 WireGuard 的方法；
> 4. [与配置 WireGuard 有关的 Ubuntu 命令](4.与配置%20WireGuard%20有关的%20Ubuntu%20命令.md)，介绍一些 Ubuntu 命令；
> 5. [多平台访问  WireGuard](5.多平台访问%20Wireuard.md) ，介绍不同平台如何使用  WireGuard ；
> 6. [付费测试  WireGuard](6.付费测试%20WireGuard.md) ，帮你试用  WireGuard 。

## 1. WireGuard 组网的三个主要构成

1. **虚拟专用网**，在两台或更多电脑上，分别安装 WireGuard ，配置相应的网络参数，就可以把它们组建成一个虚拟专用网，互相之间能够用内网地址 Ping 通；
2. **公钥/私钥**，为每台电脑生成一组公钥与私钥，并合成到虚拟网卡中，使虚拟内网中两台电脑间的通信具有私密性；
3. **路由转发**，在原先就能够科学上网的电脑启动 ip 转发功能，并设置相应的路由规则，使虚拟内网的其它电脑能够通过这台电脑全部实现科学上网。

## 2. WireGuard 组网的主要步骤

组建一个 WireGuard 网络一般需要以下步骤。

1. 在两台电脑安装 WireGuard ，安装过程简单，但需要注意查看是否有出错提示并做相应的处理。
2. 生成两台电脑各自的公钥与私钥。并准备好服务器的 IP 地址 、端口号、内网地址、公钥与私钥备用；准备好本地机的公钥与私钥备用。
3. 准备好以上参数就可以进行组网及参数配置，WireGuard 提供了命令行与配置文件两种不同的实现方式，这两种方式实现的功能相同，只是具体操作方法上有些区别。推荐先用命令行方式做测试，测试通过后再使用配置文件的方式。
   - [x] 配置文件的方式，就是通过编辑配置文件（ 比如 wg0.conf ）设置各种参数，然后通过  `WireGuard` 自带的 `wg-quick`  命令来启动或关闭  `WireGuard` 网络。好处是操作方便，不足就是调试阶段不够方便（其实也不难）。详细介绍见：“ [2.用 wg-quick 调用 wg0.conf 管理 WireGuard](2.用%20wg-quick%20调用%20wg0.conf%20管理%20WireGuard.md) ” 。
   - [ ] 命令行方式，就是把组网命令一行一行输入，完成组网。主要通过 `WireGuard` 自带的 `wg` 命令与系统自有的 `ip` 及 `iptables` 命令实现。命令行方式灵活，但如果重复操作或繁琐一些，详细介绍见：“ [3.用 wg 在命令行运行 WireGuard](3.用%20wg%20在命令行运行%20WireGuard.md) ” 。
4. 配置与测试又可以分两步进行，第一步先把内网调试通过，然后再进行第二部数据转发的配置，当基本的组网完成之后，两台电脑的内网地址互相之间应该都能 Ping 通。
5. 然后就可以进行数据转发的配置，需要在服务器端做两项配置：
   - 以配置文件方式或命令行方式设置服务器的路由规则，把数据流转到虚拟网卡。
   - 在服务器端启动 IP 转发（ 使 `net.ipv4.ip_forward=1`）；

这时应该能从本地机 Ping 通 Google.com ，并能通过浏览器访问 Google.com 。

一个实例见：[1.用 Linode 主机搭建 WireGuard 网络](1.用%20Linode%20主机搭建%20WireGuard%20网络.md) 。

## 3. 参考链接

1. WireGuard 官网：https://www.wireguard.com/
2. 官方 Android 版：https://play.google.com/store/apps/details?id=com.wireguard.android
3. wg-quick 命令官方介绍：https://git.zx2c4.com/WireGuard/about/src/tools/man/wg-quick.8
4. wg 命令官方介绍：https://git.zx2c4.com/WireGuard/about/src/tools/man/wg.8
5. Windows 系统运行的 WireGuard 客户端（同时提供免费可用服务器）：https://tunsafe.com/
6. 内置 WireGuard 的虚拟主机：https://www.azirevpn.com
7. 内置 WireGuard 的虚拟主机：https://mullvad.net/zh-hant/
8. WireGuard 配置和上网流量优化： https://blog.mozcp.com/wireguard-usage/
9. WireGuard 介绍及客户端使用教程：[https://medium.com/@xtarin/wireguard](https://medium.com/@xtarin/wireguard%E4%BB%8B%E7%BB%8D%E5%8F%8A%E5%AE%A2%E6%88%B7%E7%AB%AF%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B-2ae1eb4bf670)
10. Linode 提供的 WireGuard 安装教程：https://www.linode.com/docs/networking/vpn/set-up-wireguard-vpn-on-ubuntu/
11. 搭建 WireGuard VPN Server：https://marskid.net/2018/09/20/wireguard-vpn-set-up/
