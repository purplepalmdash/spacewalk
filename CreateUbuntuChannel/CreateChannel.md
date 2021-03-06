## 创建频道
### 创建父频道
在频道 -> 管理软件频道 下，点击"创建频道":    

![/images/2015_09_15_18_05_20_668x235.jpg](/images/2015_09_15_18_05_20_668x235.jpg)    

在弹出的窗口中，按以下设置，创建一个Debian 64位的频道:    

![/images/2015_09_16_17_44_10_454x380.jpg](/images/2015_09_16_17_44_10_454x380.jpg)    

接着往下，设置频道的可访问属性:    

![/images/2015_09_15_18_09_29_461x336.jpg](/images/2015_09_15_18_09_29_461x336.jpg)    

在频道中，可以看到ubuntu1404已经被加入到Spacewalk服务器中:     

![/images/2015_09_15_18_10_51_791x245.jpg](/images/2015_09_15_18_10_51_791x245.jpg)    

### 创建子频道
Ubuntu一般需要4个频道: base,updates,security和backports, base即我们上面创建的父频道, 
子频道的创建， 我们以trusty-updates为例说明Ubuntu子频道的建立， trusty是Ubuntu14.04的代号。   

如下图所示，建立trusty-updates频道：    

![/images/2015_09_16_17_48_04_521x344.jpg](/images/2015_09_16_17_48_04_521x344.jpg)   

注意设置这个频道的可访问属性，和父频道一样。   

创建完毕后，在频道列表可以看到新创建的子频道:    

![/images/2015_09_15_18_52_34_667x270.jpg](/images/2015_09_15_18_52_34_667x270.jpg)   
      
依次，创建其他两个频道。最后的频道列表如下:    

![/images/2015_09_15_18_55_06_786x169.jpg](/images/2015_09_15_18_55_06_786x169.jpg)         
