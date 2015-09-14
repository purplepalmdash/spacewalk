## 基本概念
### PXE

严格来说，PXE 并不是一种安装方式，而是一种引导的方式。进行 PXE 安装的必要条件是要安装的
计算机中包含一个 PXE 支持的网卡（NIC），即网卡中必须要有 PXE Client。PXE （Pre-boot
Execution Environment）协议使计算机可以通过网络启动。协议分为 client 和 server 端，PXE
client 在网卡的 ROM 中，当计算机引导时，BIOS 把 PXE client 调入内存执行，由 PXE client
将放置在远端的文件通过网络下载到本地运行。运行 PXE 协议需要设置 DHCP 服务器和 TFTP 服务
器。DHCP 服务器用来给 PXE client（将要安装系统的主机）分配一个 IP 地址，由于是给 PXE
client 分配 IP 地址，所以在配置 DHCP 服务器时需要增加相应的 PXE 设置。此外，在 PXE
client 的 ROM 中，已经存在了 TFTP Client。PXE Client 通过 TFTP 协议到 TFTP Server 上下
载所需的文件。

### KickStart

KickStart是一种无人职守安装方式。KickStart的工作原理是通过记录典型的安装过程中所需人工
干预填写的各种参数，并生成一个名为ks.cfg的文件；在其后的安装过程中（不只局限于生成
KickStart安装文件的机器）当出现要求填写参数的情况时，安装程序会首先去查找KickStart生成
的文件，当找到合适的参数时，就采用找到的参数，当没有找到合适的参数时，才需要安装者手工
干预。这样，如果KickStart文件涵盖了安装过程中出现的所有需要填写的参数时，安装者完全可以
只告诉安装程序从何处取ks.cfg文件，然后去忙自己的事情。等安装完毕，安装程序会根据ks.cfg
中设置的重启选项来重启系统，并结束安装。

### PXE + Kickstart

执行 PXE + KickStart安装需要的设备为：
* DHCP 服务器；
* TFTP 服务器；
* KickStart所生成的ks.cfg配置文件

### Cobbler

手动配置网络Kickstart安装环境的步骤并不简单，需要部署人员熟悉各种不同服务器的配置，而且
，用Kickstart部署出来的客户端机器，管理起来可能也会比较麻烦。机器数量增加到一定程度时，
因为彼此配置文件的差异，可能导致许多元素变得混乱。    

Cobbler是Redhat支持的一个开源项目，用来部署和安装系统。    

Cobbler被设计成一个Linux安装服务器，用于基于网络的快速安装和部署系统。它粘合了许多Linux
任务，并使其自动化，这样安装人员不需要在诸多繁琐的命令之中浪费时间，基于已有的部署方案
，很容易修改出适应新环境的方案。Cobbler有助于部署、管理DNS和DHCP，更新包、电源管理、配
置管理等场合。    

项目主页在[http://cobbler.github.io/](http://cobbler.github.io/)    

由于项目篇幅的关系，在这里对Cobbler不做深入的了解。    

### Cobbler与Spacewalk
从0.4版本开始，Spacewalk添加了操作系统安装功能，可以使用Cobbler Linux安装服务器用于部署
和管理系统。      

Spacewalk原生支持RHEL/CentOS/Fedora/OpenSuse的安装。      
