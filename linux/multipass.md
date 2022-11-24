# Multipass
>Multipass is a tool to generate cloud-style Ubuntu VMs quickly on Linux, macOS, and Windows.

跨平台生成云风格的虚拟机。


## 创建一个定制化的实例
> Run the command multipass find. This shows us a list of all of the images we can launch through Multipass currently.

`multipass find`, 显示当前可用的镜像。

> Let’s launch an instance running Ubuntu 22.10 (“kinetic”) by typing the command  `multipass launch kinetic`.

`multipass launch kinetic` 启动一个ubuntu 22.10 实例。

挂载文件夹
`multipass mount source target`

```shell
## 启动一个虚拟机示例
multipass launch -n demo  -c 1 -m 1G -d 10G 
## 查看已启动实例列表：
multipass list
## 停止、启动、删除实例：
multipass stop foo
multipass start foo
multipass delete foo
multipass purge foo
```