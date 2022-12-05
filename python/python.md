## 基础
### 输入输出

### 数字
* 四舍五入
```
from decimal import Decimal, ROUND_HALF_UP
Decimal(str()).quantize()
```

### 字符串

### 多线程
单 用 threading.Lock()
多 用 queue
* [python之os.exec*族用法简结](https://www.cnblogs.com/johnyang/p/10888780.html)
> 基本的'v'型exec，参数是可执行程序的名字，如‘c:/python.exe’,'D:/java.exe'等和用来运行程序用的命令参数组成的列表或者元组。

* [python 中 os.fork() 与 os.waitpid(pid，*args, **args) 说明](https://blog.csdn.net/zhupenghui176/article/details/109097737)

* [僵尸进程](https://baike.baidu.com/item/%E5%83%B5%E5%B0%B8%E8%BF%9B%E7%A8%8B/1036577?fr=aladdin)
    * 僵尸进程的危害
        占用进程号
    * 产生僵尸进程的原因
        Unix 为保证父进程知道子进程结束时的状态信息，会在子进程结束时，保留一些状态信息，并向父进程发送信号。
        SIGCHLD就是内核在任何一个进程终止时发送给父进程的一个信号。
        子进程，比父进程先结束，且父进程没有等待（回收资源， wait/waitpid）它，那么它将变成一个僵尸进程。
    * 防止僵尸进程的方法：
        1. 父进程wait，父进程会阻塞
        2. 父进程，为子进程注册signalhandler
        3. 父进程，对SIGCHLD 忽略。


### 迭代 推导
* 列表推导
[ x + 10 for x in L]
列表推导，需要更少的代码量，且运行速度大大提升。
迭代在解释器内是以C语言的速度执行的。
* 扩展的列表推导语法
    if 
    for

### 虚拟环境
虚拟环境，我记得有好几种方案
> nvim 提示 没有设置 `virtual-venv`

pyvenv 是 Python 3.3 和 3.4 中创建虚拟环境的推荐工具，不过 在 Python 3.6 中已弃用。
现在推荐使用 venv 来创建虚拟环境。

venv
```shell
# 此时建立的是 python3.9的虚拟环境
# 安装不同版本的python3 用 不同版本的python3解释器执行即可
python3.9 -m venv venv-ark
source venv-ark/bin/activate
```


[如何安装不同版本的python3](https://askubuntu.com/questions/682869/how-do-i-install-a-different-python-version-using-apt-get)
```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.5 python3.5-dev python3.5-venv
```

* venv的使用




### json
[Python中json模块的load/loads方法实战及参数详解](https://blog.csdn.net/daerzei/article/details/100598901)


### IDE
* vscode 调试
    [使用VS Code调试Python程序](https://zhuanlan.zhihu.com/p/41189402)
    [vscode python配置及调试环境搭建](https://zhuanlan.zhihu.com/p/393302498)


## 系统编程
### 持久化存储
将内存中的数据存储在文件中
### epoll
[linux 高并发网络编程之epoll详解](https://blog.csdn.net/mengzuchao/article/details/81294554)
[linux之epoll](https://www.cnblogs.com/xiaobingqianrui/p/9234290.html)
[python使用epoll实现服务端的方法](https://www.jb51.net/article/148945.htm)
[epoll(4) - Linux man page](https://linux.die.net/man/4/epoll)
[socket --- 底层网络接口](https://docs.python.org/zh-cn/3.8/library/socket.html)
多次运行一个示例，且每次执行之间等待时间过短，可能导致这个错误:`OSError: [Errno 98] Address already in use`
要防止这种情况，需要设置一个 socket 标志 socket.SO_REUSEADDR:
[python epoll socket创建服务器](https://blog.csdn.net/m0_55415810/article/details/121694738)



## 问题
1. [No module named 'Tkinter'](https://stackoverflow.com/questions/25905540/importerror-no-module-named-tkinter)
    sudo apt-get install python3-tk

2. [跨文件夹引用(表哥/弟模块)](https://zhuanlan.zhihu.com/p/144044213)
```shell
#A.py
import sys
#跨文件目录
sys.path.append('..')
from lib import B.py
```
`path = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))`

3. [Set Window Icon in Tkinter](https://www.delftstack.com/howto/python-tkinter/how-to-set-window-icon-in-tkinter/)
此问题暂时未解决

4. 线程结果不随机
```python
    def create_threads(self):
        for i in range(4):
            threading.Thread(target=self.producer, args=(i,)).start()
```

5. 连续赋值，导致无法迭代
    



## 学习资源
[python doc](https://docs.python.org/zh-cn/3.8/)
[python programming resource](http://www.rmi.net/~lutz/about-pp4e.html)

