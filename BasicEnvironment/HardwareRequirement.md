## 硬件需求
Spacewalk网页上没有提供对硬件参数的需求，但我们可以参考Satellite 5.7对硬件的需求:    
[https://access.redhat.com/documentation/en-US/Red_Hat_Satellite/5.7/html/Installation_Guide/sect-Hardware_Requirements.html#x86_64_Hardware_Requirements](https://access.redhat.com/documentation/en-US/Red_Hat_Satellite/5.7/html/Installation_Guide/sect-Hardware_Requirements.html#x86_64_Hardware_Requirements)    

### Satellite 5.7官方推荐参数

CPU

    最小 : Intel 单核处理器， 2.4GHZ, 512K Cache或相当
    推荐 : Intel 多核处理器， 2.4GHZ双核， 512K Cache或相当

内存

    最小 : 4 GB
    推荐 : 8 GB或更多

存储

    5 GB磁盘空间用于基础安装
    每个软件频道最小占用40 GB磁盘空间（包括基础频道和子频道），在/var/satellite/
    最小10 GB磁盘空间用于缓存文件存储，位于/var/cache/rhn 
    强烈推荐： SCSI硬盘，Raid5 

数据库

    本机数据库： 需要至少12 GB空间，位于/opt/rh/postgresql92/root/var/lib/pgsql/

### 实际需求
实际搭建过程中，Spacewalk可以运行在两核，3 GB内存的虚拟机上，磁盘空间在80 GB以上就足以
支持一个以上的软件频道。    

物理主机的配置可以参考：    

CPU： I3 以上，双核四线程以上即可。    
内存： 至少4 GB，其中3 GB用于分配给Spacewalk虚拟机，剩余1 GB以上给节点机。推荐升级到8
GB以上。    
磁盘： 至少160 GB剩余空间。    
网络： Internet. 联网以获取各种更新。     
