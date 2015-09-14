## 管理软件频道
Spacewalk在创建CentOS7频道时，已经默认创建好了库。使用库可以同步额外的软件包到现有的频
道，库可以被链接到一个或多个频道。     

库可以通过频道->管理软件频道->管理库来进行管理，如下图:     

![/images/2015_09_10_09_48_25_503x489.jpg](/images/2015_09_10_09_48_25_503x489.jpg)    

点击"Updates - CentOS 7 Updates(x86_64)" 进入到库的管理细节。    

![/images/2015_09_10_10_49_15_813x497.jpg](/images/2015_09_10_10_49_15_813x497.jpg)    

这里我们可以对库的细节做更改，例如我们可以更改URL为本地事先同步好的一个本地镜像源：     

![/images/2015_09_10_11_01_14_488x275.jpg](/images/2015_09_10_11_01_14_488x275.jpg)    

同步仓库步骤： 频道-> 管理软件频道-> CentOS7 Updates(x86_64), 选择库后，接着选择同步:    

![/images/2015_09_11_15_45_06_535x423.jpg](/images/2015_09_11_15_45_06_535x423.jpg)    

点击"现在同步"按钮，Spacewalk将马上开始同步进程，同步完成以后，刷新软件包频道，可以看到有
数千个包已经被加入了子频道。    

![/images/2015_09_11_15_48_36_654x225.jpg](/images/2015_09_11_15_48_36_654x225.jpg)    

依次类推，同步其他频道的包。     