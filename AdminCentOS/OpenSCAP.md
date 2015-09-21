## OpenSCAP
### 背景知识

NIST建立了信息安全类产品的SCAP兼容性认证机制。安全配置管理、漏洞测试和其他安全审计工具
的开发商，如果希望把其产品售往美国政府市场，需要遵照相关要求对其产品进行认证。当前美国
各大安全厂商在其企业级系统安全管理工具中均集成了对于SCAP的支持，如Symantec的SRAS（
Symantec Risk Automation Suite）以及eEye的Retina等。因此，使用商业化的工具能够很方便地
配合SCAP进行系统配置管理。

在开源领域亦有很多与SCAP 相关的项目，其中比较重要的有OpenSCAP、OVALDi以及eSCAPe等，这些
项目形成了对SCAP的一套完整的开发和利用体系，eSCAPe用于SCAPContent的生成，而OpenSCAP、
OVALDi用于执行基于SCAP的扫描。

### OpenSCAP
OpenSCAP由Redhat主导开发，是一个整合了SCAP中各标准的开源框架，其为SCAP的使用者提供了一
套简单易用的接口。OpenSCAP实现了对SCAP数据格式的解析以及执行检查操作所使用的系统信息探
针，它能够让SCAP的采纳者专注于业务实现，而不是处理一些繁琐的底层技术。目前OpenSCAP最新
版本完全支持SCAP 1.0规范中的全部标准。

### 安装
在客户机上，安装OpenSCAP:    

```
# yum install -y spacewalk-oscap
# yum install openscap-utils scap-security-guide -y
```

### scap 扫描
在系统 -> 审计 -> 调度中可以设置OpenSCAP的调度:    

![/images/2015_09_21_15_24_49_620x465.jpg](/images/2015_09_21_15_24_49_620x465.jpg)   

扫描后的结果:    

![/images/2015_09_21_15_28_14_815x347.jpg](/images/2015_09_21_15_28_14_815x347.jpg)   

更详细的说明:    
[https://access.redhat.com/documentation/zh-CN/Red_Hat_Network_Satellite/5.5/html/User_Guide/sect-Red_Hat_Network_Satellite-User_Guide-OpenSCAP-OpenSCAP_in_RHN_Satellite.html#sect-Red_Hat_Network_Satellite-User_Guide-OpenSCAP_in_RHN_Satellite-How_to_View_SCAP_Results](/https://access.redhat.com/documentation/zh-CN/Red_Hat_Network_Satellite/5.5/html/User_Guide/sect-Red_Hat_Network_Satellite-User_Guide-OpenSCAP-OpenSCAP_in_RHN_Satellite.html#sect-Red_Hat_Network_Satellite-User_Guide-OpenSCAP_in_RHN_Satellite-How_to_View_SCAP_Results)   
