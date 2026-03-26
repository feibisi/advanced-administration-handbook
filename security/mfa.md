---
title: "两步验证"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Security
    - Repos
    - Data
---

# 两步验证

也称为双因素认证。

随着越来越多的网站寻求更安全的登录方式，两步验证正在互联网上普及。登录是用户在线操作中最薄弱的环节。

### 什么是两步验证 {#what-is-two-step-authentication}

密码是网络登录的实际标准，但它们相对容易被破解。即使你设置了强密码并定期更换，它们仍需存储在你登录的任何地方，服务器一旦被攻破就可能泄露密码。识别一个人的方式有三种：你是谁、你有什么、你知道什么。

使用密码登录是单步验证，它仅依赖于“你知道什么”。顾名思义，两步验证是一种系统，它要求你使用三种可能因素中的两种来证明身份，而不仅仅是一种。然而，在实践中，当前的两步验证实现仍然依赖于“你知道的”密码，但会使用你的手机或其他设备通过“你拥有的”东西进行验证。

#### 三种可能的因素 {#three-possible-factors}

有三种可能的方式来识别用户。

**你拥有的特征**

每个用户都有许多独特的属性可用于识别身份。最常用的是指纹，但视网膜、声音、DNA或任何其他个人特有的特征都适用。这被称为生物识别信息，因为这些信息都属于一个人的生物学特征。

生物识别因素很有趣，因为它们不易伪造，用户也永远不会丢失或忘记它们。然而，生物识别认证很棘手，因为丢失的指纹永远无法替换。如果黑客能够访问指纹数据库，用户将无法重置或获取新的指纹。

2013年，苹果发布了TouchID，允许用户使用指纹解锁iPhone。这项技术的有趣之处在于，指纹存储在手机本地，而不是存储在云端，从而降低了黑客窃取的风险。这种方法仍然存在权衡，但它是迄今为止最广泛的消费者生物识别认证应用。

**你拥有的物品**

也称为拥有因素，用户可以通过他们携带的设备来识别。传统上，公司想要启用两步认证时，会向用户分发安全密钥链。密钥链每30秒显示一个新数字，用户每次登录时都需要输入该数字和密码。

现代的两步认证更频繁地依赖用户的智能手机，而不是新的硬件设备。一种常见的模式是使用短信来提供简单的第二因素。当用户输入密码后，他们会收到一条包含唯一代码的短信。通过输入该代码，他们被认为证明了他们也拥有自己的手机。不幸的是，短信并不是安全的通信渠道，因此开发了智能手机应用程序和[插件](#plugins-for-two-step-authentication)来创建安全通道。

**你知道的信息**

最熟悉的认证形式是知识因素，即密码。密码的历史可以追溯到[芝麻开门](https://en.wikipedia.org/wiki/Open_sesame)，长期以来一直是匿名认证的标准。为了使知识因素起作用，双方都需要知道密码，但其他方必须无法找到或猜测它。

第一个挑战是与可信方安全地交换密码。在网络上，当你注册一个新网站时，你的密码需要发送到该网站的服务器，并可能在此过程中被拦截（这就是为什么在注册或登录时应该始终检查SSL——[HTTPS](https://developer.wordpress.org/advanced-administration/security/https/)）。

密码一旦接收，必须保密。用户不应将其写下或在其他地方使用，网站也需谨慎保护数据库，以防黑客获取密码。

最后，密码需要验证。用户访问网站时，需提供密码并与存储副本核对。此过程也可能被截获（因此应始终通过 SSL — [HTTPS](https://developer.wordpress.org/advanced-administration/security/https/) 进行），并给用户带来另一风险。

#### 优势 {#benefits}

提升网站安全性的方法有很多，但 WordPress 安全团队[曾指出](https://wpvip.com/security/)："您在线操作中最薄弱的安全环节就是密码"，因此投入精力加强网站的这一方面是明智之举。

#### 缺点 {#drawbacks}

顾名思义，两步验证为原本可能已冗长繁琐的流程增加了一个步骤。尽管如今大多数高安全性的登录都采用两步验证保护，但多数消费级应用即便提供此功能，也仅将其作为可选选项。这是因为如果操作更困难，用户注册和登录服务的意愿就会降低。

两步验证也可能阻碍合法登录。如果用户将手机忘在家中且启用了两步验证，他们将无法访问自己的账户。这正是智能手机对两步验证特别有用的主要原因之一——用户随身携带手机的可能性远高于其他物品。

#### 两步验证插件 {#plugins-for-two-step-authentication}

您可以在 [WordPress.org 插件库中搜索两步验证插件](https://wordpress.org/plugins/tags/two-factor-authentication)。以下是一些最受欢迎的插件供您入门（按字母顺序排列）：

* [Duo](https://wordpress.org/plugins/duo-wordpress/)
* [Google Authenticator](https://wordpress.org/plugins/google-authenticator/)
* [Rublon](https://wordpress.org/plugins/rublon/)
* [Two-Factor](https://wordpress.org/plugins/two-factor/)
* [WordFence](https://wordpress.org/plugins/wordfence/)

### 相关 {#related}

* [暴力破解攻击](https://developer.wordpress.org/advanced-administration/security/brute-force/)