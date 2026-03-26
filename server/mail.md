---
title: "邮件"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Server
    - Repos
    - Data
---

# 邮件

本页面详细阐述了为 WordPress 设置本地邮件环境的理念、操作流程及推荐实践。

## 先决条件

开始之前，您需要对 [SMTP 协议](https://en.wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) 以及三种邮件代理（[MUA](https://en.wikipedia.org/wiki/Mail_user_agent)、[MTA](https://en.wikipedia.org/wiki/Message_transfer_agent) 和 [MDA](https://en.wikipedia.org/wiki/Message_delivery_agent)）各自的角色有基本的了解。

## WordPress 的邮件发送理念

WordPress 默认从不提供 MUA 或 MTA。它仅提供一个语言 API 库的封装器，该库负责将消息提交给能与 PHP `mail()` 函数或 `SMTP` 扩展交互的 MTA。

这意味着，服务器管理员有责任正确配置 MTA 及相关邮件环境，以便与所选的语言 API 交互。历史上，[PHPMailer](https://github.com/PHPMailer/PHPMailer) 一直是首选库。WordPress 不参与发送邮件的过程，仅参与格式化消息以提交给此邮件 API 的过程。

## WordPress 默认邮件系统的工作原理

目前，WordPress 使用 `wp_mail()` 包装函数。在默认配置下，该包装器指示 PHPMailer 库使用 PHP 内置的 `mail()` 函数，这要求系统存在一个充当 MTA 的 sendmail 兼容二进制文件。因此，要让 WordPress 开箱即用，需要正确配置相关的本地邮件环境。

如果您使用的是托管服务，这通常不是问题。但如果您运行自己的服务器，并且没有设置 SMTP 服务器（MTA）和/或未正确配置相关邮件环境，则外发邮件将无法发送，除非您将 WordPress 的包装器配置为使用远程 SMTP 服务器。

## 推荐操作

除了技术层面，根据不同的场景概述推荐的解决方案也很有价值，这样管理员就不会选择不适合自己的方案。

如果你的 WordPress 站点使用托管主机，通常无需额外操作即可发送邮件，但可能需要采取一些额外措施来确保送达率，如下文所述。从 WordPress 安装中发出的邮件应该可以开箱即用。所有与邮件相关的问题都应咨询你的网络主机，你可能需要查看主机是否提供通过其管理面板配置 SMTP 中继的选项。

如果你使用的是自托管平台（即运行自己的服务器），通常有三种方式可以设置邮件服务：

- **设置本地 MTA 或专用邮件服务器直接发送邮件**。使用此方法，管理员需负责一切事宜。请注意，即使你正确配置了所有相关软件，仍可能遇到问题，因为邮件送达率还涉及你无法控制的因素，例如 IP 信誉。由于需要大量开销和持续维护，此方法通常仅适用于有相关经验的服务器管理员，因为可能会遇到严重的送达率问题。

- **设置本地 MTA 并将其邮件中继到第三方邮件服务**。这是大多数自托管设置的推荐方法，因为它比前一种方法更容易实现，并且仍然与 WordPress 的理念兼容。如果你不想处理运行完整 MTA 时邮件送达率问题的复杂性，这是理想选择。事实上，一些小型网络主机也采用此方法（他们会持有第三方邮件服务的商业账户，并将所有邮件中继到邮件服务）。如果你自行托管和管理站点，并通过 WordPress 发送大量邮件，此方法也很理想，因为你可以将责任委托给第三方邮件服务。与第一种方法一样，请注意此选项并非没有风险，如下一节所述。即使依赖第三方邮件服务，设置 MTA 始终需要一定程度的服务器管理专业知识。

- **通过代码配置 SMTP 中继以利用第三方邮件服务**。如果你不处理大量邮件，或者不想处理运行自托管 MTA 时邮件送达率问题的复杂性，这是推荐的方法。请注意，此方法[会导致性能下降](https://konstantin.blog/2021/wp_mail-is-not-broken/)，原因有很多。

## 设置您自己的 MTA

如果您选择设置自己的 MTA（无论是用于本地还是第三方中继），本节将简要介绍在 *NIX 和 Windows 环境中设置 MTA 的基础知识。在深入细节之前，请注意这种方法存在一些风险。必须开放端口 25，如果不小心处理可能会带来问题，因为这是最常见的攻击途径之一。如果未能妥善保护 SMTP 中继身份验证，您可能会面临安全问题，或者服务器被用于发送垃圾邮件。

### *NIX 服务器基础

如果你运行自己的 *NIX 服务器，你的机器上应该已安装最流行的 MTA（邮件传输代理），如 `postfix`、`exim`、`qmail` 或 `sendmail`，只需进行配置即可（你可以在网上搜索相关教程）。若希望使用第三方邮件服务，推荐的做法是设置本地 MTA 将邮件转发至远程 SMTP 服务器，以避免页面加载时间受到影响。

### Windows 主机服务器特定设置

检查 SMTP 虚拟服务器上的“中继”设置。授予 `127.0.0.1` 访问权限。然后在 `php.ini` 文件中，将 `SMTP` 设置设为相同的 IP 地址。同时将 `smtp_port` 设为 `25`。

在 Windows 机器上，可以尝试使用 [支持 TLS v1.2 的 Windows 版 Fake sendmail](https://github.com/sendmail-tls1-2/main) 等 sendmail 模拟器。

### MTA 设置的替代方案

如果您不想在服务器上手动设置完整的 MTA，还有其他一些选择：

* 使用中继 MTA 替代本地全功能 MTA：例如，[msmtp-mta](https://wiki.debian.org/msmtp) 是一个简化的 MTA，可用于通过远程 SMTP 服务器（即远程 MTA）转发电子邮件。该软件包提供了指向 `msmtp` 的 `/usr/sbin/sendmail` 符号链接，并具有其他软件可直接使用的 sendmail 兼容接口。

* 使用 SMTP 插件：您可以从 WP.org 插件目录安装 [SMTP 插件](https://wordpress.org/plugins/search/smtp/)。这类插件会覆盖 `wp_mail()` 的默认设置，使用 PHPMailer 或其他自选的邮件库，这些库利用远程第三方邮件服务代表您的域名发送邮件。

### 确保邮件可送达性

当我们的系统开始发送邮件后，需要确保邮件能实际送达收件人。有几项内容需要检查：

#### 确保使用正确的发件地址

默认情况下，WordPress 封装器会将 `From:` 字段填充为 `wordpress@example.com`，发件人名称设为 *WordPress*。如果这是一个有效的电子邮件地址，这样设置没有问题。例如，如果你的域名是 `example.com`，你的主机将传递 `wordpress@example.com` 进行投递。只要 `example.com` 已设置收发邮件，即使 `wordpress` 不是有效的邮箱，你的邮件也可能会被发送。一些旧的邮件投递代理会检查 `From:` 地址是否有效且能接收邮件。如今这种检查已不常见，但我们建议确保使用有效的电子邮件地址，并且该邮箱存在于你的接收邮件服务器中。如果你使用默认的 `From:` 地址，只需验证你可以在 `wordpress@example.com` 邮箱中接收邮件。请记住，`example.com` 是你的 WordPress 安装域名。

如果你不希望从与安装域名相同的域发送邮件，则必须在 WordPress 中进行一些代码更改。例如，如果你的 WordPress 域名是 `example.org`，但你想从 `mail.example.org` 发送邮件，邮件可能无法发送，因为 `example.org` 的 DNS 记录可能未授权你的邮件服务器处理邮件，因为默认的 `From:` 地址将是 `wordpress@wordpress.org`。要更改默认的 `From:` 地址，你的 WordPress 可以[使用 `wp_mail_from` 过滤器](https://developer.wordpress.org/reference/hooks/wp_mail_from/)。对于名称，你可以使用 [`wp_mail_from_name` 过滤器](https://developer.wordpress.org/reference/hooks/wp_mail_from_name/)。

你也可以在配置的 MTA 服务器中强制设置 `From:` 地址配置。请参考你的 MTA 相关文档。

#### 回复地址

WordPress 封装器会忽略 `Reply-To:` 字段，因此它将与 `From:` 地址保持一致。按照上述示例，默认情况下为 `wordpress@example.com`。如果您希望用户能够回复发送的邮件，请确保此邮箱存在于您的接收邮件服务器上。默认的 `Reply-To:` 地址始终与 `From:` 地址相同，除非您在邮件头中明确将其设置为不同的地址。

仅有一种情况会自动设置 `Reply-To:` 地址：当我们向评论文章的用户发送邮件时。在这种情况下，`Reply-To:` 地址将被设置为评论者明确指定的电子邮件地址。

### 排查垃圾邮件送达问题

您的邮件可能被路由至垃圾邮件文件夹，甚至更糟，直接被当作恶意邮件丢弃。您可以采取几项措施来说服收件方的邮件服务器，证明您的邮件是合法的，应按指定地址投递。设置 SPF、DKIM 和 DMARC 记录是确保送达率的关键步骤。网上有多种指南可帮助您设置这些记录。以下将解释这些记录为何有益。

#### SPF（发件人策略框架）

这是最常用的反垃圾邮件措施。邮件投递代理（MDA）会检查发件人域的 SPF 记录，以确认其是否被授权为该域发送邮件。SPF 是一种 TXT 类型的 DNS 记录，您需要自行添加或检查主机商是否已为您添加，该记录列出了被授权为该域发送邮件的 IP 地址或主机名。这确保了邮件并非来自未经授权的来源。例如，如果某个未知服务器 IP 地址发送了一封在 `From:` 字段中包含您域名的电子邮件，借助 SPF DNS 记录，MDA 会将其作为垃圾邮件拒收。但如果您未在 DNS 记录中正确配置此设置，您的邮件也可能被当作垃圾邮件拒绝。

请记住，如果您通过不同服务器（包括您的 WordPress 安装）发送电子邮件，则需要将所有地址添加到 SPF 记录中。

#### DKIM（域名密钥识别邮件）

这种反垃圾邮件的完整性验证检查在过去几年中获得了广泛认可，并已被大多数主要电子邮件提供商实施。您可以在同一封邮件中同时使用 SPF 和 DKIM。同样，与 SPF 类似，您可以通过检查邮件头来验证接收邮件服务器是否验证了您主机的域名密钥。与 SPF 一样，如果您可以编辑 TXT DNS 记录，您可以自行设置 DKIM 凭证。如果您处于托管环境中，请咨询您的托管提供商是否已为您设置此功能，因为这是常见做法。

如果您需要自行管理，有多种方法可以在邮件中发送正确的 DKIM 签名：

* 配置您的 MTA 以使用 DKIM 签名签署邮件。
* 某些 SMTP 插件提供 DKIM 签名选项。
* 使用 `PHPMailer` 库设置 DKIM 签名。为此，您可能需要[挂载到 `phpmailer_init` 操作](https://developer.wordpress.org/reference/hooks/phpmailer_init/)并遵循 PHPMailer 库的文档（此方法无官方文档，请参阅[此帖子了解更多详情](https://stackoverflow.com/a/24464694/4442122)）。

同时，请确保使用正确的域名信息生成 DKIM 密钥，并相应地发布 DNS 记录。当您将 DKIM 公钥添加到 DNS 记录后，MDA 将检查邮件头的 DKIM-Signature 并验证签名。如果签名无效，MDA 会将邮件作为垃圾邮件拒绝。这旨在防止电子邮件被伪造并从未经授权的来源发送。DKIM 和 SPF 结合使用，以确保邮件的合法性并按地址正确投递。

#### DMARC (Domain-based Message Authentication, Reporting & Conformance)

The DMARC record is another TXT DNS record that you need to add or check if your hosting has added it for you, that lists the actions to take when the email has not passed the SPF or DKIM checks. DMARC is becoming almost mandatory, so to ensure deliverability, we recommend setting it up. You can read more about it [here](https://dmarc.org/).