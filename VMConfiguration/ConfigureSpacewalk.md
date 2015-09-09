## 配置Spacewalk
Spacewalk可以通过手动或者通过answer文件来配置，为了简单起见，我们这里采用answer文件进行
配置:     
应答文件内容如下:     

```
# vi /root/answer.txt
admin-email = root@localhost
ssl-set-org = Spacewalk Org
ssl-set-org-unit = spacewalk
ssl-set-city = My City
ssl-set-state = My State
ssl-set-country = US
ssl-password = spacewalk
ssl-set-email = root@localhost
ssl-config-sslvhost = Y
db-backend=postgresql
db-name=spaceschema
db-user=spacewalk
db-password=spacewalk
db-host=localhost
db-port=5432
enable-tftp=N
```
使用预定义好的answer文件设置spacewalk:    

```
# spacewalk-setup --disconnected --answer-file=/root/answer.txt
```

配置完毕后，访问`https://10.17.20.2`来访问spacewalk的web页面. 你将看到下面的页面:    
![/images/2015_09_09_16_23_10_724x508.jpg](/images/2015_09_09_16_23_10_724x508.jpg)   

创建用户，用户名/密码分别为admin/spacewalk123。     
![/images/2015_09_09_16_25_12_812x562.jpg](/images/2015_09_09_16_25_12_812x562.jpg)    

登入后页面如下:    
![/images/2015_09_09_16_26_30_747x491.jpg](/images/2015_09_09_16_26_30_747x491.jpg)    
在iptables中打开如下端口:    

```
# iptables -A INPUT -p tcp -m multiport --dport 22,443,5222,69,5432 -j ACCEPT 
```
