## 如何连接主机内的虚拟机(kvm)
> When you deploy a virtual machine in KVM, you connect the virtual machine into the default virtual network, acting as a NAT interface. As a result, your VM will be able to go out to the internet and get the updates. Later, when you try to connect to the VM using SSH or other means, it will not let you connect. The default NAT connection will only allow outbound connection from the Virtual machine, not any inbound traffic, because it is configured with NAT.

当部署虚拟机的时候，若使用默认虚拟网络，VM的流量可以出去，但无法从其它主机通过ssh连接vm。
将 vm 部署到本地局域网。

### 为什么要部署到host network
> And from the LAN perspective, if you were to sniff the traffic, you would see the traffic coming from the host machine itself rather than the VM. 

在NAT模式下，包来自`host`，而不是`vm`。

> But the problem with this type of network is that you cannot connect to your virtual machine from your LAN or from your host machines where the virtual machines are running.

问题在于，无法从局域网访问虚拟机。

>  no one can talk to the virtual machine because inbound access is not allowed.

NAT下，没有人可以访问`VM`，

> We need to use a bridge interface for the Local network and the host machine to connect to the virtual machine.

因此，我们需要桥接到局域网。

### 什么是 KVM 中的桥接模式
What is a bridge interface in KVM?
>  The bridge interface is like a virtual layer2 switch, where packets/frames are transmitted using MAC addresses.

桥接模式，像一个二层的虚拟机，用MAC地址传输帧。

> After you create a bridge interface in the Linux machine, you need to map that bridge interface to a physical interface of the host machine. 

创建虚拟网桥后，将其映射到物理网卡。

在写这篇笔记的时候，我似乎已经有一个网卡是桥接模式了。
`ovs-br0`

### 实践
#### 安装 bridge-utils
`sudo apt-get install bridge-utils -y`

* 当执行`brctl show`时，STP是什么意思？
    `SPANNING TREE PROTOCOL`生成树协议。
    作用：找到最短路径，并消除拓扑结构中的循环。

#### 配置Ubuntu kvm 中的桥接网卡
1. 找到物理网卡名称
    enx00e19900a0f7
    192.168.5.207
2. 备份 netplan 配置
    `sudo cp ./01-network-manager-all.yaml ./01-network-manager-all.yaml.bak`
3. 更新 bridge 中的 netplan配置
    根据自己的环境调整配置。
    `sudo netplan apply`
    配置
```
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    enx00e19900a0f7:
      dhcp4: no
  bridges:
    test:
      addresses: [192.168.5.207/24]
      gateway4: 192.168.5.1
      nameservers:
        search: [Local]
        addresses: [8.8.8.8, 4.2.2.2]
      interfaces:
        - enx00e19900a0f7
```
    __暂停进行__
    这种做法或许可以，但有点不太符合实际
    也符合，之后还是要连接laptop中的虚拟机的。
    电脑作为radius服务器,
    出师未捷身先死，长使英雄泪满襟
4. 


#### 验证桥接网络适配器

#### 将 VM 连接到网桥

#### 测试连通性



### 参考资料
[How to Connect KVM VM to Host Network?](https://getlabsdone.com/how-to-connect-kvm-vm-to-host-network/)

## 更换kvm 创建 bridge 网络的方式
[KVM: Creating a bridged network with NetPlan on Ubuntu 22.04](https://fabianlee.org/2022/09/20/kvm-creating-a-bridged-network-with-netplan-on-ubuntu-22-04/#switchtonetplan)

### 更换network manage 为 networkd
1. Look at existing NetPlan
```
network:
  version: 2
  renderer: networkd
  ethernets:
    enx00e19900a0f7:
      dhcp4: false
      dhcp6: false
      addresses: [192.168.5.207/24]
      routes:
      - to: default
        via: 192.168.5.1
        metric: 100
        on-link: true
      mtu: 1500
      nameservers:
        addresses: [8.8.8.8]
```
2. Created a bridge with Netplan
备份
```
cd /etc/netplan

# make backup
sudo cp 50-cloud-init.yaml 50-cloud-init.yaml.orig

# modify, add bridge
sudo vi 50-cloud-init.yaml
```
创建网桥
```
network:
  version: 2
  renderer: networkd

  ethernets:
    enp1s0:
      dhcp4: false 
      dhcp6: false 

  bridges:
    br0:
      interfaces: [enp1s0]
      addresses: [192.168.1.239/24]
      # gateway4 is deprecated, use routes instead
      routes:
      - to: default
        via: 192.168.1.1
        metric: 100
        on-link: true
      mtu: 1500
      nameservers:
        addresses: [8.8.8.8]
      parameters:
        stp: true
        forward-delay: 4
      dhcp4: no
      dhcp6: no
```

### Configure libvirt network to use existing bridge
```
<!-- file host-bridge.xml -->
<network>
  <name>host-bridge</name>
  <forward mode="bridge"/>
  <bridge name="br0"/>
</network>
```
创建网络
```shell
# current list of libvirt networks
virsh net-list

# create libvirt network using existing host bridge
virsh net-define host-bridge.xml
virsh net-start host-bridge
virsh net-autostart host-bridge

# state should be active, autostart, and persistent
virsh net-list --all
```

### KVM Guest OS
最后一步，配置KVM虚拟机使用网桥

### 参考资料
[KVM: Creating a bridged network with NetPlan on Ubuntu 22.04](https://fabianlee.org/2022/09/20/kvm-creating-a-bridged-network-with-netplan-on-ubuntu-22-04/#switchtonetplan)
还是不行，虚拟机内部的桥接使用网络
* 猜测原因
  * [ ] 虚拟机创建方式的不同
    逐步验证，确保操作并没有出错。
    在本机重新创建虚拟尝试
  * [ ] ubuntu版本的不同
