# IP
NAME
: ip - show / manipulate routing, network devices, interfaces and tunnels

SYNOPSIS
: ip [ OPTIONS ] OBJECT { COMMAND | help }

## route
查看 ip route 说明文档`man ip-route`。

The 'ip route' command is a global configuration command. This command defines a static route for a specific destination network. To define a static route, this command needs the following information.
`ip route`是一个全局的配置命令，它需要以下信息:
* The destination network address
    目的网络地址路由
* Either the name of the local interface connected to the destination network or the IP address of the next-hop router.
    要么是连接目的网络的网卡名字(直连)，要么是 下一跳路由的IP
* Additional control options

### The 'ip route' command syntax
> The 'ip route' command uses two syntaxes. It uses the first syntax to specify the local interface to forward data packets to the destination network and the second syntax to specify the IP address of the next-hop router that can send data packets to the destination network.

`ip route` 有两种语法，一种指定直连网卡，一种指定下一跳IP地址。

> If you specify the local interface, the router assumes the destination network is directly connected to the local interface and forwards the destination network's data packets to the destination network. If you specify the IP address of the next-hop router, the router assumes the destination network is available on another router. In this case, the router sends the destination network's data packets to the next-hop router. A next-hop router is a router that knows how to reach the destination network.

如果你指定网卡，`ip route`默认直连，若指定IP地址，默认下一跳router。
下一跳router知道如何抵达目标网络。

To specify the name of the local interface, uses the following syntax.
```shell
Router(config)# ip route destination_network_# [subnet_mask]
interface_to_exit
[administrative_distance] [permanent]
```
To specify the IP address of the next-hop router, uses the following syntax.
```shell
Router(config)# ip route destination_network_# [subnet_mask]
ip_address_of_next_hop_neighbor
[administrative_distance] [permanent]
```

### Let's understand the difference between both options.
You can connect multiple networks to the same interface of the router. If you connect more than one networks to the same interface, the interface works as a multi-access link. If the interface is connected to only one network, the interface works as a point-to-point link.
一张网卡多个网络，一张网卡一个网络
On a multi-access link, you should always use the IP address of the next-hop.
在`multi-access`中使用IP address
On a point-to-point link, you can use any one option from both options.
在`p2p`中使用interface

### 修改静态路由
`ip route add default via 192.168.1.1 dev eth0`
`ip route add 10.1.1.0/30 encap mpls 200/300 via 10.1.1.1 dev eth0`

原默认路由为
`default via 192.168.5.1 dev enx00e19900a0f7 proto dhcp metric 100`

``` shell
sudo ip r del default via 192.168.5.1 dev enx00e19900a0f7
sudo ip r add default via 192.168.122.1 dev virbr0
```



### 参考文章
[IP Route command Explained with Examples](https://www.computernetworkingnotes.com/ccna-study-guide/ip-route-command-explained-with-examples.html#:~:text=The%20%27ip%20route%27%20command%20is,command%20needs%20the%20following%20information.&text=Either%20the%20name%20of%20the,of%20the%20next%2Dhop%20router.)

