## CentOS勘误
### 准备
CEFS: CentOS Errata for Spacewalk, 可以用于往Spacewalk中导入CentOS勘误。    

[http://cefs.steve-meier.de/](http://cefs.steve-meier.de/)    

下载最新的Errata XML文件, oval文件, errata导入脚本:    

```
#  mkdir CEFS
# cd CEFS/
# wget http://cefs.steve-meier.de/errata.latest.xml
# wget https://www.redhat.com/security/data/oval/com.redhat.rhsa-all.xml
# wget http://cefs.steve-meier.de/errata-import.tar 
```

### 导入勘误
导入勘误的步骤如下:    

```
# tar xf errata-import.tar
# chmod 755 errata-import.pl
# export SPACEWALK_USER='admin'
# export SPACEWALK_PASS='spacewalk123'
# ./errata-import.pl --server spacewalkrootnode --errata ./errata.latest.xml \
  --rhsa-oval ./com.redhat.rhsa-all.xml --channel centos7-x86_64 \ 
  --os-version 7 --publish 
```
引入勘误是个漫长的过程，大约耗时30分钟～几个小时不等。    

引入完毕后，勘误一栏看起来如下:    

![/images/2015_09_18_16_25_57_706x553.jpg](/images/2015_09_18_16_25_57_706x553.jpg)    

### 使用勘误
导入勘误完成以后，在系统中可以看到系统列表下已经显示了勘误的数量和严重程度:    

![/images/2015_09_18_16_27_36_631x146.jpg](/images/2015_09_18_16_27_36_631x146.jpg)    

点击系统，选择我们需要引入的勘误后， 点击"应用勘误"：    

![/images/2015_09_18_16_29_22_589x547.jpg](/images/2015_09_18_16_29_22_589x547.jpg)    

选择应用时间后开始引入勘误:    

![/images/2015_09_18_16_30_43_584x373.jpg](/images/2015_09_18_16_30_43_584x373.jpg)  

在系统 -> 事件下将看到勘误的导入过程和历史:    

![/images/2015_09_18_16_32_57_590x493.jpg](/images/2015_09_18_16_32_57_590x493.jpg)   

有的勘误在完成之后需要重启，在Spacewalk中直接重启即可。    
