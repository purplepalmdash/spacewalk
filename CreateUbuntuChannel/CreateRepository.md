## 创建仓库
手动创建的频道并不会在创建的时候同时创建仓库，我们需要手动创建出Ubuntu所需的四个仓库。
仓库里含有所有软件安装包，注册到Spacewalk服务器的系统将从仓库取回包以进行本地安装。    

下面以trusty-base为例，讲解仓库的创建。

### 创建trusty-base仓库
在菜单： 频道 -> 管理软件频道 -> 管理库, 点击"创建存储库":    

![/images/2015_09_15_19_03_30_787x237.jpg](/images/2015_09_15_19_03_30_787x237.jpg)    

在弹出的窗口中，写入如下参数:    

![/images/2015_09_15_19_06_46_442x337.jpg](/images/2015_09_15_19_06_46_442x337.jpg)   

注意这里我们用了本地同步好的一个库以便加速同步过程，如果你的外网连接速度够快，你可以换
用最近的公网源。    

依次创建好trusty-backport, trusty-updates, trusty-security三个软件仓库。   

创建好的仓库列表如下：   

![/images/2015_09_15_19_09_52_691x129.jpg](/images/2015_09_15_19_09_52_691x129.jpg)    

### 链接频道与仓库
在菜单： 频道 -> 管理软件频道, 选择我们刚才添加的ubuntu1404频道，我们需要把这个频道(事
实上它是trustybase频道)， 链接到我们刚才添加的trusty-base软件仓库。    

选中该频道后，点击"库"， 在下面的标签"添加/删除", 勾上"trusty-base":    

![/images/2015_09_15_19_13_47_370x284.jpg](/images/2015_09_15_19_13_47_370x284.jpg)   

勾中软件仓库后，点"更新库", 这样就完成了频道与仓库的链接。    

![/images/2015_09_15_19_14_41_787x294.jpg](/images/2015_09_15_19_14_41_787x294.jpg)   

依次完成剩下的3个频道与软件仓库的链接。   

### 同步软件包
遗憾的是，第一次同步软件包不能通过图形界面来进行，为此我们使用以下步骤来进行包的同步:    

修改下列文件中的定义，以支持更多扩展:    

```
# vim /usr/lib/python2.7/site-packages/debian/debfile.py
- PART_EXTS = ['gz', 'bz2', 'xz']   # possible extensions
+ PART_EXTS = ['gz', 'bz2', 'xz', 'lzma']   # possible extensions
```

下载同步仓库所需的perl脚本:    

```
# mkdir ~/Code
# yum install -y git
# git clone https://github.com/stevemeier/spacewalk-debian-sync.git
# cd spacewalk-debian-sync/
# mv spacewalk-debian-sync.pl /usr/bin/
# chmod 777 /usr/bin/spacewalk-debian-sync.pl
# yum install -y  perl-WWW-Mechanize
```

创建用于同步ubuntu1404频道的脚本:    

```
# cd ~/Code/
# mkdir Sync
# cd Sync/
# vim ubuntu1404_sync.sh
# chmod 777 ubuntu1404_sync.sh
```

`ubuntu1404_sync.sh`文件内容如下:    

```
until spacewalk-debian-sync.pl --username admin --password spacewalk123 --channel \
'ubuntu1404' --url 'http://192.168.0.79/ubuntu/dists/trusty/main/binary-amd64/'
 do
         echo "do it again"
         sleep 15
 done
```

运行结果如下:   

```
./ubuntu1404_sync.sh 
INFO: Repo URL: http://192.168.0.79/ubuntu/dists/trusty/main/binary-amd64/
INFO: Ubuntu root is http://192.168.0.79/ubuntu/
INFO: Fetching Packages.gz... done
INFO: Packages in repo:         8566
INFO: Packages already synced:  0
INFO: Packages to sync:         8566
INFO: 1/8566 : checkbox-ng-service_0.3-2_all.deb
INFO: 2/8566 : liblingua-en-inflect-perl_1.895-1_all.deb
```
同步所需时间比较长。大约需要1个小时。    

### 其他同步脚本
updates同步脚本:    

```
until spacewalk-debian-sync.pl --username admin --password spacewalk123 --channel \
'trusty-updates' --url \
'http://192.168.0.79/ubuntu/dists/trusty-updates/main/binary-amd64/'
 do
         echo "do it again"
         sleep 15
 done

``` 

backports同步脚本:    

```
until spacewalk-debian-sync.pl --username admin --password spacewalk123 --channel \
'trusty-backports' --url \
'http://192.168.0.79/ubuntu/dists/trusty-backports/main/binary-amd64/'
 do
         echo "do it again"
         sleep 15
 done
```

security同步脚本:    

```
until spacewalk-debian-sync.pl --username admin --password spacewalk123 --channel \
'trusty-updates' --url \
'http://192.168.0.79/ubuntu/dists/trusty-updates/main/binary-amd64/'
 do
         echo "do it again"
         sleep 15
 done
```

经过漫长的同步以后，最终同步后的结果如下:    
