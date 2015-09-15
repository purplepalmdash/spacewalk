## 运行命令
### 开启功能
"运行远程命令"使得我们可以在Spacewalk服务器端远程执行客户端命令。默认加入的系统最初是没
有开启远程命令功能的。需要加入privisioning属性后才可以。    


![/images/2015_09_15_11_19_14_442x246.jpg](/images/2015_09_15_11_19_14_442x246.jpg)    

开启provisioning:    

![/images/2015_09_15_12_28_15_564x405.jpg](/images/2015_09_15_12_28_15_564x405.jpg)    

开启完毕后，远程命令选项出现:    

![/images/2015_09_15_11_19_29_587x264.jpg](/images/2015_09_15_11_19_29_587x264.jpg)

### 运行远程命令
在CentOS 7客户端机上需要安装rhncfg-actions包，并打开允许运行，这样才能支持远端执行命令:    

```
# yum install -y rhncfg-actions
# rhn-actions --enable-run
```

远程执行命令的界面:    

![/images/2015_09_15_14_10_05_543x430.jpg](/images/2015_09_15_14_10_05_543x430.jpg)    

远程执行`uptime`:    

![/images/2015_09_15_14_13_30_496x366.jpg](/images/2015_09_15_14_13_30_496x366.jpg)   

运行结果可以在系统的历史事件中看到:    

![/images/2015_09_15_14_24_55_644x543.jpg](/images/2015_09_15_14_24_55_644x543.jpg)    