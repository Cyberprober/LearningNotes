## brctl
brctl - ethernet bridge administration

将不同的局域网连接起来作为一个网使用。

## 实例
The command __brctl addbr <name>__ creates a new instance of the ethernet bridge. The network interface corresponding to the bridge  will be called <name>.

The  command  __brctl delbr <name>__ deletes the instance <name> of the ethernet bridge. The network interface corresponding to the  bridge must be down before it can be deleted!

The command __brctl show__ shows all current instances of the ethernet bridge.

## Ports
__brctl addif <brname> <ifname>__ will make the interface <ifname> a port of the bridge <brname>. 
__brctl delif <brname> <ifname>__ will detach the interface <ifname> from the bridge <brname>.
__brctl show <brname>__ will show some information  on  the bridge and its attached ports.

## Ageing
建立路由表, 哪个ip从哪个端口来

## spanning tree protocol
生成树协议， 防止有环路

