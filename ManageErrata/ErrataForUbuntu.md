## Ubuntu勘误

### 准备
我们可以使用philicious创建好的脚本，具体过程可以参照：    

[http://www.devops-blog.net/spacewalk/configuring-errata-for-ubuntu-with-spacewalk](http://www.devops-blog.net/spacewalk/configuring-errata-for-ubuntu-with-spacewalk)   

```
# git clone https://github.com/philicious/spacewalk-scripts /opt/spacewalk-errata/
# cd /opt/spacewalk-errata/
# mkdir errata
```

### 导入勘误
我们需要对下载的脚本做一点点小小的修改，以引入勘误:    

```
# chmod a+x *.sh
# chmod a+x *.pl
# chmod a+x *.py
# vim errata-import.py
login = "admin"
passwd = "spacewalk123"
```

修改spacewalk-errata.sh文件如下:    

```
#!/bin/bash

# Processes Ubuntu Errata and imports them to Spacewalk

#make sure we have english locale
export LC_TIME="en_US.utf8"
# Obtains the current date and year.
#DATE=`date +"%Y-%B"`
DATE=$1
# Fetches the errata data from ubuntu.com.
rm -rf /opt/spacewalk-errata/errata/$DATE.txt
rm -rf /opt/spacewalk-errata/errata/ubuntu-errata.xml
wget -P  /opt/spacewalk-errata/errata
https://lists.ubuntu.com/archives/ubuntu-security-announce/$DATE.txt.gz
gunzip -f /opt/spacewalk-errata/errata/$DATE.txt.gz
# Processes and imports the errata.
cd /opt/spacewalk-errata/ && \
/opt/spacewalk-errata/parseUbuntu.py errata/$DATE.txt && \
cp ubuntu-errata.xml errata/ubuntu-errata.xml
/opt/spacewalk-errata/errata-import.py >> /var/log/ubuntu-errata.log
rm -f ubuntu-errata.xml
rm -f errata/ubuntu-errata.xml
```

这样修改出来的spacewalk-errata.sh可以接受我们手工填入的日期参数，我们手工传入不同的年月
，以获取所有的errata更新:    

```
# cat allyear.sh
./spacewalk-errata.sh 2015-September
./spacewalk-errata.sh 2015-July
./spacewalk-errata.sh 2015-June
./spacewalk-errata.sh 2015-May
./spacewalk-errata.sh 2015-April
./spacewalk-errata.sh 2015-March
./spacewalk-errata.sh 2015-February
./spacewalk-errata.sh 2015-January
./spacewalk-errata.sh 2014-December
./spacewalk-errata.sh 2014-November
./spacewalk-errata.sh 2014-October
./spacewalk-errata.sh 2014-September
./spacewalk-errata.sh 2014-August
./spacewalk-errata.sh 2014-July
./spacewalk-errata.sh 2014-June
./spacewalk-errata.sh 2014-May
./spacewalk-errata.sh 2014-April
./spacewalk-errata.sh 2014-March
./spacewalk-errata.sh 2014-February
./spacewalk-errata.sh 2014-January
```

运行`./allyear.sh`可以下载到任意指定日期内的Errata更新。      

### 结果
运行完毕以后，系统可以看到对应的更新：    

![/images/2015_09_18_18_11_09_628x167.jpg](/images/2015_09_18_18_11_09_628x167.jpg)   

点击可以看到更为详细的勘误内容:    

![/images/2015_09_18_18_12_29_539x381.jpg](/images/2015_09_18_18_12_29_539x381.jpg)   

### 应用勘误
在客户机需要安装`errata.py`文件:    

```
$ wget https://github.com/spacewalkproject/spacewalk/blob
/master/client/rhel/yum-rhn-plugin/actions/errata.py
$ sudo mv errata.py /usr/share/rhn/actions
$ sudo chmod 777 /usr/share/rhn/actions/errata.py
```

在Spacewalk web界面中选中需要被应用的勘误，下发给系统后，就可以在系统的历史事件中看到勘
误被应用的全过程。    

![/images/2015_09_18_18_23_15_688x341.jpg](/images/2015_09_18_18_23_15_688x341.jpg)   
