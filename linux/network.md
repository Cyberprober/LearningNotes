## ubuntu 从 networkManager 变为 network
`systemctl restart systemd-networkd.service`
`systemctl restart systemd-resolved.service`



## 网络问题
* 无法访问路由器
    * 重启 netplan apply
    * 重启 networkd
    * 查看 brctl show
    * 重新获取ip dhco
    * 可能是网线松了

* 可以访问路由器，却不可以访问114
    可能是 clash 开启了防DNS污染    




