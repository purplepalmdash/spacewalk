## 安装Spacewalk
### 配置仓库
为了获得更好的下载速度，我们将使用aliyun提供的CentOS和epel仓库来进行安装, 以下是更改和
配置仓库的具体步骤。    

```
# cd /etc/yum.repos.d/
# mkdir Back
# mv *.repo Back/
# curl http://mirrors.aliyun.com/repo/Centos-7.repo>/etc/yum.repos.d/CentOS-Base.repo
# curl http://mirrors.aliyun.com/repo/epel-7.repo>/etc/yum.repos.d/epel.repo
```
我们需要移除掉epel仓库中关于cobbler*的定义, 通过增加exclude的选项:     

```
# vim /etc/yum.repos.d/epel.repo 
[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
baseurl=http://mirrors.aliyun.com/epel/7/$basearch
        http://mirrors.aliyuncs.com/epel/7/$basearch
#mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch
failovermethod=priority
enabled=1
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
exclude=cobbler*
```
引入spacewalk仓库:    

```
# rpm -Uvh http://yum.spacewalkproject.org/2.3/RHEL/7/x86_64/spacewalk-repo-2.3-4.el7.noarch.rpm
```
引入jpackage-generic仓库，这个仓库提供了额外的依赖。    

```
cat > /etc/yum.repos.d/jpackage-generic.repo << EOF
[jpackage-generic]
name=JPackage generic
#baseurl=http://mirrors.dotsrc.org/pub/jpackage/5.0/generic/free/
mirrorlist=http://www.jpackage.org/mirrorlist.php?dist=generic&type=free&release=5.0
enabled=1
gpgcheck=1
gpgkey=http://www.jpackage.org/jpackage.asc
EOF
```

### 安装包
安装以下包, 截至到写书时默认安装的是2.3版本, 大概需要下载300M 左右的安装包:     

```
# yum install -y spacewalk-postgresql spacewalk-setup-postgresql \ 
spacecmd spacewalk-utils firefox
# yum groupinstall 'X Window System' -y
```

添加中文字体支持:    

```
# yum install -y wqy-zenhei-fonts.noarch wqy-unibit-fonts.noarch \
wqy-microhei-fonts.noarch
```
