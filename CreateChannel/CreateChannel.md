## 创建频道
Spacewalk中已经预定义好CentOS7的频道，使用如下命令创建之, 需要注意替换用户名和密码为我
们之前设定的(admin/spacewalk123)：    

```
# /usr/bin/spacewalk-common-channels -v -u admin -p spacewalk123 \
 -a x86_64 -k unlimited  'centos7*'
```
现在点击Spacewalk网页里的频道条目就可以看到我们添加的频道了，如下图:    

![/images/2015_09_09_17_03_42_818x504.jpg](/images/2015_09_09_17_03_42_818x504.jpg)    

新创建好的频道是空的，没有任何软件包，接下来一节我们将添加CentOS7的安装包到频道里。  
