## Spacewalk介绍

### Spacewalk维基百科介绍页

Spacewalk介绍, 翻译自维基百科Spacewalk (Software) 页面
([https://en.wikipedia.org/wiki/Spacewalk_%28software%29](https://en.wikipedia.org/wiki/Spacewalk_%28software%29))：    

Spacewalk是Red Hat公司提供的一个开源（GPLv2）Linux系统管理解决方案。它的前身是Red Hat
Satellite项目衍生出来的上游社区项目, 于2008年开放源代码。Spacewalk项目包括了Web界面和后
端，以及Red Hat Proxy Server（红帽代理服务器）以及与Satellite相关的客户端软件，用户和开
发者可以在在一个自由和开源软件许可证FOSS（free and open source software）的条款下使用它
。Spacewalk项目和Satellite项目之间的关系可以类比于Fedora和Red Hat Enterprise Linux之间
的关系。Satellite仅能管理RHEL和Solaris系统，而Spacewalk则可以管理Fedora, CentOS, Suse及
Debian系统。

Spacewalk由遵循GNU通用公共许可证版本2的软件组成。最初使用商业Oracle数据库作为后端，从
1.7版（发布于2012年3月）开始提供了对PostgreSQL数据库的支持。

Satellite 5.3是Spacewalk代码基于的第一个上游版本。 

2011年3月Novell发布的SUSE管理器1.2，基于Spacewalk1.2, 同时提供对SUSE Linux Enterprise和Red Hat企业版Linux的管理。

截至2015年，Spacewalk和Red Hat Satellite都已成为红帽长期的支持模式, 借鉴于Red Hat
Network的经验，它们都被设计成了整体式工具, 但主要用于内部部署。其设计标准为类似于
OpenStack的以网络为中心
的云计算平台， 类似于Chef和Puppet的配置和管理工具。红帽选择了开源组件（包括了Puppet和
Foreman）作为Satellite 6的基础，Satellite 6已于2014年9月发布。

### Spacewalk更多信息

属性 | 参数     
-----|------
作者 | Red Hat
初版发行 | 2008年6月
稳定版 | 2.3/2015年4月14日
开发语言 | Java, Perl和Python
操作系统 | Linux
语言 | English, Francais, Bengali, Hindi, Japanese, Punjabi, Russian, Simplified Chinese, German, Spanish, Gujarati, Italian, Korean, Brazilian, Portuguese, Tamil, Traditional Chinese
类型 | 系统管理
许可证 | GNU General Public License v2
官方网站 | http://www.spacewalkproject.org/

### Spacewalk特性一览

Spacewalk的重要特性：
* 系统硬件和软件信息清单
* 在你的系统上安装和更新软件
* 收集和发布你的自定义软件包到管理组
* 准备（通过kickstart）你的系统
* 管理和部署配置文件到你的系统
* 监控你的系统
* 准备和启动/停止/配置虚拟客户机
* 跨多个地理位置高效地分发内容
