> **原文链接**: https://mp.weixin.qq.com/s?__biz=MzkzMTcwMTg1Mg==&mid=2247492152&idx=1&sn=3df639947dc621517ce0fbecccf483b7

#  金蝶云星空 AppDesignerService.RecordCurDevCodeInfo.common.kdsvc远程代码执行  
Superhero  Nday Poc   2025-07-15 02:38  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCkYN4sZibCVo6EFo0N9b7Kib4I4N6j6Y10tynLOdgov9ibUmaNwW5yeoCbQ/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCkhic5lbbPcpxTLtLccZ04WhwDotW7g2b3zBgZeS5uvFH4dxf0tj0Rutw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
![图片](https://mmbiz.qpic.cn/mmbiz_png/Melo944GVOJECe5vg2C5YWgpyo1D5bCk524CiapZejYicic1Hf8LPt8qR893A3IP38J3NMmskDZjyqNkShewpibEfA/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
内容仅用于学习交流自查使用，由于传播、利用本公众号所提供的  
POC  
信息及  
POC对应脚本  
而造成的任何直接或者间接的后果及损失，均由使用者本人负责，公众号Nday Poc及作者不为此承担任何责任，一旦造成后果请自行承担！  
  
  
**01**  
  
**漏洞概述**  
  
  
金蝶云星空   
  
Kingdee.BOS.ServiceFacade.ServicesStub.AppDesigner.AppDesignerService.RecordCurDevCodeInfo.common.kdsvc  
接口处存在远程代码执行漏洞,未经身份验证的远程攻击者可利用此漏洞执行任意系统命令，写入后门文件，获取服务器权限。  
  
**02******  
  
**搜索引擎**  
  
  
360quake:  

```
(app:&#34;金蝶&#34;) AND title: &#34;金蝶云星空 管理中心&#34;
```

  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwIqg7uGMCzrSqWYy5b9xhbib8u7icXDdBDqAgpRI7HC68LpzHSzEyEoqkz4VbmZIFibpfibfS8PCeyX5A/640?wx_fmt=png&from=appmsg&watermark=1&wxfrom=5&wx_lazy=1&tp=webp "")  
  
  
**03******  
  
**漏洞复现**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwIiatjn92ibdNm7P5AS4m3E8Yrw5kmrsPrAwgeuAeN90jGcAeUFiauThnF7UMWjDWRXMPudeg9GfhODg/640?wx_fmt=png&from=appmsg "")  
  
  
**04**  
  
**自查工具**  
  
  
nuclei  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwIiatjn92ibdNm7P5AS4m3E8YK0STx7icJKX0gsJjO6hujLUJzJkonGrYhARWpEDlEH2tooW0nRH3qIg/640?wx_fmt=png&from=appmsg "")  
  
afrog  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwIiatjn92ibdNm7P5AS4m3E8YX9ZxEbX4DkNseIvymyjvryEWjY0a5OZqGQNWkTOhjWic2BC1sUsXtkA/640?wx_fmt=png&from=appmsg "")  
  
xray  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwIiatjn92ibdNm7P5AS4m3E8YnA5Gxq0kaG035MsygwHX8IDibPbppjOnwSdhInVoBtWOalhGalUUUCA/640?wx_fmt=png&from=appmsg "")  
  
  
**05******  
  
**修复建议**  
  
  
1、关闭互联网暴露面或接口设置访问权限  
  
2、升级至安全版本  
  
  
**06******  
  
**内部圈子介绍**  
  
  
【Nday漏洞实战圈】🛠️   
  
专注公开1day/Nday漏洞复现  
 · 工具链适配支持  
  
 ✧━━━━━━━━━━━━━━━━✧   
  
🔍 资源内容  
  
 ▫️ 整合全网公开  
1day/Nday  
漏洞POC详情  
  
 ▫️ 适配Xray/Afrog/Nuclei检测脚本  
  
 ▫️ 支持内置与自定义POC目录混合扫描   
  
🔄 更新计划   
  
▫️ 每周新增7-10个实用POC（来源公开平台）   
  
▫️ 所有脚本经过基础测试，降低调试成本   
  
🎯 适用场景   
  
▫️ 企业漏洞自查 ▫️ 渗透测试 ▫️ 红蓝对抗   
▫️ 安全运维  
  
✧━━━━━━━━━━━━━━━━✧   
  
⚠️ 声明：仅限合法授权测试，严禁违规使用！  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/wnJTy44dqwIqg7uGMCzrSqWYy5b9xhbibuYpAwY60IMKfszmhnVRniay6MawgEkjsSyHahULEHwXr6LJjAAliaO3A/640?wx_fmt=png&from=appmsg&watermark=1&wxfrom=5&wx_lazy=1&tp=webp "")  
  
  
