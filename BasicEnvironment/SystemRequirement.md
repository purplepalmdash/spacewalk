## 系统需求
我们将使用虚拟机来部署Spacewalk服务器和运行集群。所以在宿主机上我们需要安装虚拟化软件并
进行相应配置。    

首先说明一下几个概念：
* 宿主机: 安装虚拟化软件的计算机，即我们使用的物理机器。
* 虚拟机: 使用虚拟机工具构建并运行的机器。    
* 宿主操作系统: 物理机上安装的操作系统，例如在物理机上安装Ubuntu/CentOS或者Windows。
* 客户操作系统：虚拟机上运行的操作系统。

我们使用CentOS 7 X86_64 作为宿主操作系统。系统的安装和配置不在本节的讨论范畴。需要确保的是虚拟
化软件的安装。    

### 虚拟化
#### 硬件虚拟化
(VT): Intel Virtualization Technology就是以前众所周知的“Vanderpool”技术，这种技术让可以让一
个CPU工作起来就像多个CPU并行运行，从而使得在一部电脑内同时运行多个操作系统成为可能。它
最早由Intel发布于2007年03月04日。 

(VMX)： 带有虚拟技术的处理器具有额外的指令集，叫做Virtual
Machine Extensions，简称VMX。VMX给CPU带来了10个新的虚拟专用指令：VMPTRLD, VMPTRST,
VMCLEAR, VMREAD, VMWRITE, VMCALL, VMLAUCH, VMRESUME, VMXOFF and VMXON。 

(AMD-V): AMD Virtualization , 别名 ： Pacifica. 它是在AMD公司制造并销售的一个嵌入CPU中运行
支持多个操作系统并行运行的硬件水平的虚拟化技术。以Pacifica的开发编码名被熟知.   

(SVM): AMD虚拟技术指令集。    

#### Hypervisor
Hypervisor是一种运行在物理服务器和操作系统之间的中间软件层,可允许多个操作系统和应用共享
一套基础物理硬件，因此也可以看作是虚拟环境中的“元”操作系统，它可以协调访问服务器上的所
有物理设备和虚拟机，也叫虚拟机监视器（Virtual Machine Monitor）。    

常见的Hypervisor包括： 
*    Hyper-V
*    KVM
*    Parallels
*    QEMU
*    VirtualBox

#### KVM
KVM 是 基于内核的虚拟机(Kernel Based Virtual Machine)的缩写， 它可以提供基于硬件虚拟化
扩展从而在一台物理机上运行多台虚拟机。它支持多种客户操作系统，诸如Linux, Windows,
Solaris, Haiku, REACT OS等等。    

KVM是集成在Linux内核中，是X86架构且硬件支持虚拟化技术（Intel VT或AMD-V）的
Linux的全虚拟化解决方案。

KVM 可以通过命令行来管理，也可以通过图形化界面来管理。

#### Qemu
Qemu是一个广泛使用的开源计算机仿真器和虚拟机。

当作为仿真器时，可以在一种架构(如PC机)下运行另一种架构(如ARM)下的操作系统和程序。通过动态转化，可以获得很高的运行效率。

当作为一个虚拟机时，qemu可以通过直接使用真机的系统资源，让虚拟系统能够获得接近于真机的性能表现。qemu支持xen或者kvm模式下的虚拟化。当用kvm时，qemu可以虚拟x86、服务器和嵌入式powerpc，以及s390的系统。

#### Libvirt
Libvirt 是一个虚拟化 API 和虚拟机(VMs)管理后台，支持远程或本地访问，支持多种虚拟化后端 (QEMU/KVM， VirtualBox， Xen，等等)

#### Virt-Manager
Virt-Manager(Virtual Machine
Manager) 是一个很广泛被使用的用于管理虚拟机的应用程序，它支持创建、编辑、启动和关
闭基于KVM的虚拟机，并支持在不同宿主机之间进行虚拟机的在线迁移和冷迁移。    


### 软件需求
我们需要确保virt-manager、libvirtd、qemu被正确安装和配置，接下来我们才可以开始进行
Spacewalk服务器的安装和配置。       

首先确保硬件的虚拟化参数被打开。在CentOS 7的终端下，运行以下命令:    

```
[root:~]# egrep '(vmx|svm)' /proc/cpuinfo
```
![/images/2015_09_01_17_27_13_653x228.jpg](/images/2015_09_01_17_27_13_653x228.jpg)    

如果看到高亮的vmx或者svm输出，则代表CPU支持硬件虚拟化。如果没有看到，则需要确认在BIOS里
是否打开了处理器虚拟化选项，或者CPU是否支持虚拟化技术。    

安装KVM:    

```
# yum install qemu-kvm qemu-img virt-manager libvirt libvirt-python libvirt-client \
virt-install virt-viewer
```
软件说明:
* qemu-kvm =  QEMU 模拟器
* qemu-img = QEMU 磁盘管理器
* virt-install =  用于命令行创建虚拟机的工具
* libvirt = 提供libvirtd 守护进程用来管理虚拟机和控制hypervisor
* libvirt-client  = 提供客户端的API，用于访问服务器端，同时提供virsh组件， 这是一个通过
命令行管理虚拟机的接口
* virt-viewer = 图形化终端

安装完毕后，在命令行敲入

```
# virt-manager
```
或者访问Gnome菜单里的"Application –> System Tools –> Virtual Machine Manager" 即可启动
virt-manager.
