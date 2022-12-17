# OpenWRT
专为嵌入式设备(特别是无线路由器)设计的高度可扩展的Linux发行版本。
## 查看OpenWRT是如何给user分配IP
1. 查看如何分配IP
2. 能否将这一IP信息提交给Controller

## openwrt 开启WiFi
当AP有局域网接口时，默认关闭WiFi。两种方式开启WiFi:
   * 网页模式 `LuCI`
   * 命令行模式, `UCI` 
openwrt 使用的应当就是 hostapd
* 尝试查看运行进程
  通过网页版可以直接查看进程
  wifi 应当是由 hostapd 启用
  dhcp 应当是由 odhcpd 启用

[Latest Hostapd](https://forum.openwrt.org/t/latest-hostapd/105099)


## DNS and DHCP configuration /etc/config/dhcp
[DNS and DHCP configuration /etc/config/dhcp](https://openwrt.org/docs/guide-user/base-system/dhcp)

> OpenWrt uses dnsmasq and odhcpd to serve DNS/DHCP and DHCPv6 by default.

__OpenWrt 默认使用`dnsmasq`提供DNS/DHCP服务，`odhcpd`提供ipv6服务。__


如果只是看 DHCP discover, DHCP offer, DHCP request, DHCP ACK
那么好像并没有什么， 能将身份和IP联系起来的，MAC是唯一的，是否可以根据MAC确定。
MAC-user, MAC-IP

思路
* [ ] 能否让DHCP在分配IP后，上报或存储IP与MAC的映射，
  * 先搭建
* [ ] radius 上报或存储user与MAC的映射。

* 可能我需要读 hostapd 的源码，找到如何让hostapd 发送 accounting request 包含 framed-ip-address

> In addition, it has a Port Access Entity (PAE) with
 Authenticator functionality for controlling the virtual
 port authorization, i.e., whether to accept
 packets from or to the station.


## WiFi
[Wi-Fi configuration](https://openwrt.org/docs/guide-user/network/wifi/start)
* Bridged AP
    桥接AP
    修改 openwrt ip `/etc/config/network`, interface lan 配置为，主路由子网，未使用的地址
    修改 wireless 指向 network lan
* AP+STA
    允许openwrt 连上一个热点，并通过该热点，提供网络连接给所有连接(无线/有线)openwrt的设备。
    * connect to client WiFI-network.
        在上述情况下，AP会创建一个不同的子网，client 连接到这个子网。
        而，主路由对这一情况无感知，可以配置成，客路由与主路由，使用同一radio。

### FreeRADIUS
[openwrt-FreeRADIUS](https://openwrt.org/docs/guide-user/network/wifi/freeradius)

#### 在虚拟机中安装 openwrt
[Run OpenWrt on QEMU/KVM](https://www.youtube.com/watch?v=HAC7sn4orf0)
> 操作过于繁杂似乎并不具备可行性

* ssh configuration
The SSH configuration is handled by the __Dropbear__ subsystem of uci and the configuration file is located in `/etc/config/dropbear`.

应当贴出信息来源

* 改变 lan ip
[[Solved] How to correctly change router LAN’s IP address?](https://forum.openwrt.org/t/solved-how-to-correctly-change-router-lans-ip-address/21654)

> Login via SSH Edit /et c/config/network directly Run /etc/init.d/network restart Done!

> 应注意，用ip a add 或 change 修改 ip 似乎并不行，应按上文所述进行操作，即 修改 network 文件

* OpenWrt in QEMU x86-64
ESXi专为运行虚拟机、最大限度降低配置要求和简化部署而设计.
> IMG/VDI/VMDK with “-hda” switch do not work with QEMU 2.x


#### openwrt 虚拟机的配置
配置好网桥，解决一下问题:
* [x] 解决稳定性问题
  重启后，网桥不能失效
  这个大概率是可以的
  [qemu-kvm如何创建虚拟机并设置网络](https://www.yisu.com/zixun/556276.html)

* [ ] 待获取IP的虚拟机如何接入 openwrt
  在虚拟机中 建一个网卡，在外面建一个没有 dhcp的网桥，待获取ip的vm的vnet全部接入这里


### 在有线网应用802.1x
[Implementing IEEE 802.1x for Wired Networks](https://www.giac.org/paper/gcwn/891/implementing-ieee-8021x-wired-networks/113734)
[Wired 802.1X Deployment Guide](https://www.cisco.com/c/en/us/td/docs/solutions/Enterprise/Security/TrustSec_1-99/Dot1X_Deployment/Dot1x_Dep_Guide.html)
[openwrt-Setting Up Wired 802.1X](https://forum.openwrt.org/t/setting-up-wired-802-1x/4595)
在一开始的时候，`client`没有IP地址，只能和交换机通信，
* 理解交换机接口
    未授权的端口，未验证或验证未成功，不允许任何流量(请求连接网络的流量除外);
    已授权的端口，成功验证，正常。
        验证完成后，client 尚未分配IP, 此时，，应从DHCP server 处获得IP，再进行下一步的通信。


the RADIUS accounting information has exactly the information I need for the Palo Alto User-ID system.
[RadiUID: Palo Alto User-ID and RADIUS](https://packetpushers.net/radiuid/)   
看起来很有用，判断能否移至

User-ID system 仍是基于IP来配置防火墙。ephemeral IP-to-User mappings 来源于身份认证系统，如radius.

应先让有线传输正常进行，再考虑别的。

* 先确定无线的是正常的。
  1. 应连接WAN口
  2. 打开网页设置 radius server ip
  3. 登录 openwrt cmd 判断是否可以ping 通 radius server ip
  4. 若全部正常，尝试使用手机连接openwrt

* 尝试无线网失败，原因:
  * 少个开发板
    有个开发板又怎样？
    kvm没有设置桥接模式，无法访问 虚拟机
    让外部可以访问vm

* 直接在有线上进行尝试就好
  * [ ] 配置合适的openwrt虚拟机
     标准，可以访问网页，
  * [ ] 尝试在有线链接上应用openwrt


* Run OpenWRT on QEMU
[Run OpenWrt on QEMU/KVM](https://www.youtube.com/watch?v=HAC7sn4orf0)
该视频，在QEMU/KVM上安装了openwrt， 并使其可以访问控制网页
  1. 配置 openwrt 允许ssh远程访问
    [Dropbear configuration](https://openwrt.org/docs/guide-user/base-system/dropbear)

[OpenWrt in QEMU](https://openwrt.org/docs/guide-user/virtualization/qemu)

1. 安装QEMU
2. x86-64下的OpenWrt
  The x86-64 target has support for ESXi images by default.
   1. 准备
   2. 配置样例
   3. 网络配置
   4. 为OpenWrt 提供网络
   5. 接入LUCI
   6. 转发主机的端口
   7. 使用KVM igb网卡




