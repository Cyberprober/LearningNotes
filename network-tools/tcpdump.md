# TCPDUMP
a powerful command-line packet analyzer

计算机之间到底在说什么，用`tcpdump`你就可以了解。

* tcpdum 做了什么
抓包，并把它打印出来。每行一个包。
tcpdump 使用`BPF`来过滤包,

## 基操
* 端口
    port 53
    src port 80
    dest port 80
    tcp port 80
* 主机
    host 192.168.3.2
    src host 192.168.3.2
    dest host 192.168.3.2
* 逻辑运算符
    host 1.2.3.4 and port 80

* 想知道关于所抓包的更多信息，可以使用`wireshark`
    若想用wireshark分析所抓包，可以:
    1. 保存pcap 文件 并用wireshark 打开
    2. 用管道，把tcpdump的输出，转给wireshark
* 经常使用的三个选项
  * -i 选择网卡
    `sudo tcpdump -i lo`
  * -w 写入文件
    `sudo tcpdump host 8.8.8.8 -w my-packet.pcap`
  * -c 计数
    `sudo tcpdump -c 1000 -w my-packet.pcap dest port 80`
* 其它选项
  * -A 打印内容
  * -n 不用主机名，用ip
  * -e 包括`ethernet net`信息，显示MAC地址

## tcpdump 配合 wireshark
Older versions of tcpdump truncate packets to 68 or 96 bytes. If this is the case, use -s to capture full-sized packets
老版本的tcpdump会把包截到68到96字节，可以用`-s`命令来捕获全部的包。
`sudo tcpdump -s 65535 -i ens4 port 1812 -w my_pcap.pcap`

802.1x协议抓包示例



## 其它的网络管理工具
ping, dig/nslookup, ss/netstat, ifconfig,  ip, arp, ngrep, traceroute/mtr, nc, telnet, ssh, sysctl
network manager, openvpn 

## 参考资料
[Let's learn tcpdump](https://wizardzines.com/zines/tcpdump/)
