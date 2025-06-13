#  Weaxor来袭，全网沦陷？看“诺亚”如何硬刚最强勒索病毒！  
第59号安全实验室  美创资讯   2025-06-13 00:30  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/ib9aljs9x2mc2KErXn6CH8HJJP1WJD4vVjibrq7vEK1X8Hl99vHdLnEw0icqgMZFiaAabSee69uLoOMk7B3RfSsKaA/640?wx_fmt=gif&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_gif/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGico78ZF9ohyvqXHfONpTr3eRczuB5ENhNDT3FmOazCicQg3AFslStyUghA/640?wx_fmt=gif&from=appmsg "")  
  
近期，美创第59号安全实验室监测到多起高度相似的勒索病毒事件，该勒索病毒通过RDP爆破、攻击数据库服务、应用程序1DAY漏洞等方式感染受害主机。一旦得手，病毒会在主机上加密文件进行勒索，加密文件后缀包括但不限于.rox、.wxr、.wxx、.weaxor等。  
  
  
经分析，该勒索病毒为Weaxor家族  
。Weaxor家族为热门勒索病毒家族--Mallox家族的新变种。Mallox勒索病毒自2021年出现，至今已实施多起臭名昭著的勒索事件。Weaxor家族作为Mallox家族的变种，自2024年底出现后迅速取代Mallox的主流地位，在多方权威分析统计报告中，Weaxor家族一直是2025年勒索病毒占比排行榜的月度“常胜冠军”。  
  
  
  
  
  
  
**01**  
  
  
  
**Weaxor勒索病毒感染表现**  
  
  
  
  
  
  
  
  
  
  
  
  
受害主机在感染Weaxor勒索病毒后，文件会被加密成**.rox、.wxr、.wxx、.weaxor**  
等格式。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGico5FPuMu1ZyIzgwOdjMIsm6FBKU1GSVaezasFOKr51HWHD77H3CkmTYw/640?wx_fmt=png&from=appmsg "")  
  
  
同时，病毒会释放勒索信文件RECOVERY INFO.txt，勒索信内容如下：  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicoHACw7gyAd98DNrCicMHPkgX8nGnoQ5zQeGLFbEVN6FaYuRgQiblEr72A/640?wx_fmt=png&from=appmsg "")  
  
  
此外在加密器文件目录下，还会生成一个wxr.txt文件，其中记录了被勒索用户的标志信息以及受害主机信息等。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicoNX38n2pjseAbMKYZSx6p55IVMWWCtadsSMYWCxQVPTubH1QVvBO6Ag/640?wx_fmt=png&from=appmsg "")  
  
  
**02**  
  
**Weaxor勒索病毒样本分析**  
  
  
  
  
  
  
  
  
  
  
  
  
此次用于分析的Weaxor勒索病毒样本为2025年2月份编译的版本，其使用C++语言进行开发和编译。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicoRleA3JHbU44eadlUq6kx6Rz9ib7EGk4ichicalhpyZj0JJDvic9EbibyXfw/640?wx_fmt=png&from=appmsg "")  
  
  
**使用静态分析工具对样本文件进行分析，发现其行为如下：**  
  
**1. 环境检查：**  
病毒运行后，首先**检查系统的语言**  
，在特定语言环境下会直接退出程序，不进行加密勒索操作。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicodxuEyib1iczK5dm4KGymG7HxEAFFaIoSicF7QATCbubRO1OwickPe8MOiaQ/640?wx_fmt=png&from=appmsg "")  
  
  
**2. 信息收集：**  
病毒会获取操作系统、网卡、磁盘等信息。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicokoPXichJrKVXE02FnRNCy2ZS1YNTAfDSCWeaCbE6CsTFje9Bfiavicq1w/640?wx_fmt=png&from=appmsg "")  
  
  
**3. 信息外传：**  
上述信息收集完成后会写入到txt文件中，并发送至以下地址  
：  
http://193.143.1.139/Ujdu8jjooue/biweax.php  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicoB1NAq7KnAXKj6Rf8DVIuZFSZ4WTcSfTTwK0MGM0YBzLwIUgSEV06dg/640?wx_fmt=png&from=appmsg "")  
  
  
  
**4. 破坏性操作：**  
  
·**删除注册表项**  
：  
猜测可能是为了防止受害主机上这些程序被安全监控软件或者运维人员使用，造成勒索病毒无法正常进行或者勒索进程中断，确保攻击影响最大化。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGico2IpuiaUP0q4QUC5d0LAUATabxpadw7jYsoTYDIj3DnjOU1pcwE51QOw/640?wx_fmt=png&from=appmsg "")  
  
  
·**删除卷影副本**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicoiawTvnelaFcWrK0jsECm2Vr5tj8AsEQbykd1bJlcIBjRxFHE6CWqSVw/640?wx_fmt=png&from=appmsg "")  
  
  
·**设置白名单**  
：设置不进行加密的白名单文件以及文件目录  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGico1AfBaOZJFuMuB5HribiaNCeLueCmyfD63fWK0u0picVPH0gL57StbCYJA/640?wx_fmt=png&from=appmsg "")  
  
  
**5. 文件加密**  
：完成上述前置工作后，Weaxor勒索病毒样本就会开始遍历并挂载所有磁盘卷，读取文件进行**加密操作**  
，并将除白名单外的所有文件命名为.wxr后缀。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGiconsVNre8Yia9icg1BibeZEUyhljwcRdbEZAdsgKClMDU0Leg7p3TWt9eVw/640?wx_fmt=png&from=appmsg "")  
  
  
**一朝诺亚 终生免疫**  
  
  
**美创诺亚防勒索**  
  
  
  
为有效应对已知或未知勒索病毒的威胁，美创通过对大量勒索病毒的深度分析，基于零信任原则，创造性地研发出针对勒索病毒的终端防护产品——**诺亚防勒索系统**  
。诺亚防勒索在不关心漏洞传播方式的情况下，  
**可防护任何已知或未知的勒索病毒**  
。  
以下为诺亚防勒索针对Weaxor勒索病毒的实际防护效果演示：  
  
  
诺亚防勒索可通过服务端统一下发策略并更新。默认策略可保护office文档（如需保护数据库文件可通过添加策略一键保护）。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicoCOASdvMFzIxzibnGicBtAqWOecudWXDfpK1RTrezvytq8MSiaxSOcEasw/640?wx_fmt=png&from=appmsg "")  
  
**未开启「诺亚防勒索」**  
  
在test目录下，添加以下文件，当服务器感染勒索病毒，该文件被加密，增加统一的异常后缀（如.wxr），且**无法正常打开**  
。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicoZra2pB1JDnVmpQQl6mibvQDEfSQibFInoDoEFcbyiacEkibZW0UiaHFkzog/640?wx_fmt=png&from=appmsg "")  
  
**开启「诺亚防勒索」**  
  
双击执行病毒文件，当勒索病毒尝试加密被保护文件，即test目录下的文件时，  
**诺亚防勒索会立即弹出警告并拦截该行为**  
。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicofVXso9hibPkXBFEul6tKLPoCrP4jJWnnd1PibwTg6UdVribpXicc04xomg/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicof9StDVyfO890aic6AjaEOkcqbMTPjHSB8iaNIsTw5l5ZF2fI0AjaqRBQ/640?wx_fmt=png&from=appmsg "")  
  
  
检查系统，可见test  
目录下的被测试文件**未被加密**  
，**可被正常打开**  
，诺亚防勒索成功阻断了恶意软件的加密行为。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicoc2gB0yYXetuPJfODgo6YXMEpqyPXf6yBMbrGu4BMXhZoiarGYpOxNtg/640?wx_fmt=png&from=appmsg "")  
  
**开启「诺亚防勒索-堡垒模式」**  
  
为全方位守护系统文件安全，诺亚防勒索提供「堡垒模式」。堡垒模式尤其适用于ATM机等极少更新的哑终端设备。一键开启堡垒模式后，所有进入终端的可执行文件都将被阻止运行，有效阻断任何新的应用程序运行，包括**勒索软件、已知勒索病毒、未知勒索病毒、挖矿病毒**  
，达到诺亚防勒索的最强防护模式。  
  
  
在堡垒模式下，尝试执行该病毒文件，立刻被移除到隔离区，病毒运行被阻断。  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGico2eibl9rCYupIjMCZ2hsseD46zgX0ZFMOIicsyjtlk94cuD0LUsSk2icwQ/640?wx_fmt=png&from=appmsg "")  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/ib9aljs9x2mfL2Rwnich8OvGC2JYGdUGicoCoT8UqiaIwQFeac7W9ztRtHAwJBLRyO0FW7ibKdfxvz7OAOSj849sR5w/640?wx_fmt=png&from=appmsg "")  
  
  
  
  
“一朝诺亚，终生免疫”，今天，诺亚防勒索已赢得各级政府单位、医院、教育、能源和大型制造企业等众多用户的认可，成为勒索病毒洪水式侵袭环境下的首要选择。  
  
  
  
  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/pJjOS4MSntVSQRXIHuHeHvW0Klr3u9EiaeB9lxKt3WOPVv7rnRqQbicn8gCnbRzMKrsrnLdaBtVe8ZxemVBSK8XQ/640?wx_fmt=jpeg "")  
  
![](https://mmbiz.qpic.cn/mmbiz_gif/7QRTvkK2qC7IHABFmuMlWQkSSzOMicicfBLfsdIjkOnDvssu6Znx4TTPsH8yZZNZ17hSbD95ww43fs5OFEppRTWg/640?wx_fmt=gif "")  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/ib9aljs9x2mfD4buRKuU8ZMSge8IZuFiauFQ3paMGootiaYqZ5iasxia90dFfQhlWlYTQuvoScDJt6gibOTIT82aHLBA/640?wx_fmt=jpeg "ÉãÍ¼Íø_401931611_wx_¿Æ¼¼ÔÆ¼ÆËã£¨ÆóÒµÉÌÓÃ£©.jpg")  
  
[美创科技入选“勒索攻击防护技术领域代表性厂商”](https://mp.weixin.qq.com/s?__biz=MzA3NDE0NDUyNA==&mid=2650800825&idx=1&sn=88d5020b20e2241fc46feba9028b7921&scene=21#wechat_redirect)  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/ib9aljs9x2mfD4buRKuU8ZMSge8IZuFiauHDdqRibcndSysdeJj3qcYOBwGsby3IibDYib5IvZq01xdjJAeIdfntaNQ/640?wx_fmt=jpeg "ÉãÍ¼Íø_401924370_wx_3D¿Æ¼¼Ð¾Æ¬³¡¾°£¨ÆóÒµÉÌÓÃ£©.jpg")  
  
[零信任防勒索产品+保险赔付+容灾备份：全面开启勒索“韧性”防护](https://mp.weixin.qq.com/s?__biz=MzA3NDE0NDUyNA==&mid=2650795459&idx=1&sn=7ca357fd278ad86b3c15b5fff108e78a&scene=21#wechat_redirect)  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/ib9aljs9x2mfD4buRKuU8ZMSge8IZuFiaug2tDdaRRSzNQgFwQzZg3LPhvMQBnQ36m0SOztKYntBkcB5FoKHTfyA/640?wx_fmt=jpeg "ÉãÍ¼Íø_401925479_wx_ÔÆ¼ÆËã£¨ÆóÒµÉÌÓÃ£©.jpg")  
  
[「诺亚」升级｜进阶主动防御，勒索病毒持续免疫](https://mp.weixin.qq.com/s?__biz=MzA3NDE0NDUyNA==&mid=2650792381&idx=1&sn=7dbdaabf7fe6ac3bfcbcd74e519f165a&scene=21#wechat_redirect)  
  
  
![](https://mmbiz.qpic.cn/mmbiz_jpg/ib9aljs9x2mfD4buRKuU8ZMSge8IZuFiauVHTXZk3r1Xaj8CbqdedZCfLJl44Yy4VNXjscn0WPKu18bf9p4s8Gbw/640?wx_fmt=jpeg "ÉãÍ¼Íø_401860691_wx_3d¿Æ¼¼¿Õ¼ä£¨ÆóÒµÉÌÓÃ£©.jpg")  
  
[妇产专科医院遭勒索攻击破局实录｜DRCC助力核心业务极速重生](https://mp.weixin.qq.com/s?__biz=MzA3NDE0NDUyNA==&mid=2650814593&idx=1&sn=660990fb3fb964810d4dd84087a58149&scene=21#wechat_redirect)  
  
  
  
[#勒索防护]()  
  [#勒索病毒]()  
   
  
