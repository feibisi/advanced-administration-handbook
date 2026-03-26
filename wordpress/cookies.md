---
title: "Cookies"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Wordpress
    - Repos
    - Data
---

# Cookies

WordPress 使用 Cookie（存储在您计算机上的微小信息片段）来验证您的身份。有针对登录用户和评论者的 Cookie。

## 在浏览器中启用 Cookie

WordPress 使用 Cookie 进行身份验证。这意味着要登录您的 WordPress 站点，您必须在浏览器中启用 Cookie。

您可以在此处找到最流行浏览器的管理方法信息：
- [Google Chrome](https://support.google.com/chrome/answer/95647)
- [Mozilla Firefox](https://support.mozilla.org/en-US/kb/enable-and-disable-cookies-website-preferences)
- [Microsoft Edge](https://privacy.microsoft.com/en-us/windows-10-microsoft-edge-and-privacy)
- [Safari](https://support.apple.com/guide/safari/manage-cookies-and-website-data-sfri11471/mac)
- [Opera](https://help.opera.com/en/latest/web-preferences/#cookies)
- [Brave](https://brave.com/privacy/browser/)

## 用户 Cookie

用户是指在 WordPress 站点注册了账户的人。

登录时，WordPress 使用 `wordpress_[hash]` cookie 来存储您的身份验证信息。其使用范围仅限于管理后台区域 `/wp-admin/`。

登录后，WordPress 会设置 `wordpress_logged_in_[hash]` cookie，该 cookie 用于在大多数界面操作中标识您的登录状态和身份。

WordPress 还会设置一些 `wp-settings-{time}-[UID]` cookie。末尾的数字是您在用户数据库表中的唯一用户 ID。此 cookie 用于自定义您对管理界面（可能也包括主站点界面）的视图。

Cookie 的生命周期可通过 `auth_cookie_expiration` 钩子进行调整。相关示例可参阅 [what's the easiest way to stop wp from ever logging me out](https://wordpress.stackexchange.com/questions/515/whats-the-easiest-way-to-stop-wp-from-ever-logging-me-out)。

### 非版本特定数据

实际 Cookie 包含您的用户名、过期时间以及确保您拥有有效会话的*哈希*数据。*哈希*是对某些数据应用特定数学公式的结果。对于这些 Cookie，仅将您哈希密码的 4 个字符存储在 Cookie 的哈希值中。这确保无法从 Cookie 中检索您的密码。同时确保每当您更改密码时，任何 Cookie 都将失效。

WordPress 使用这两个 Cookie 来绕过 `wp-login.php` 的密码输入部分。如果 WordPress 识别到您拥有有效且未过期的 Cookie，您将直接进入 [WordPress 管理界面](https://wordpress.org/documentation/article/administration-screens)。如果您没有 Cookie，或 Cookie 已过期，或以其他方式无效（例如您手动编辑了它们），WordPress 将要求您重新登录以获取新的 Cookie。

## 评论者 Cookie

当访客在您的博客上发表评论时，他们的计算机上也会存储 Cookie。这纯粹是为了方便，以便访客在想要再次发表评论时无需重新填写所有信息。为评论者设置了三个 Cookie：

- `comment_author_{HASH}`
- `comment_author_email_{HASH}`
- `comment_author_url_{HASH}`

评论者 Cookie 的有效期设置为自设置之日起略少于一年。

## WordPress 测试 Cookie

WordPress 会设置一个名为 `wordpress_test_cookie` 的临时 Cookie，用于探测 WordPress 设置 Cookie 的能力。如果写入此 Cookie 失败，您将看到以下错误信息："您的浏览器阻止或不支持 Cookie。"

如果在迁移网站后遇到此问题，请尝试删除您的 Cookie；如果使用了缓存插件，请同时清除服务器缓存。这将解决临时性问题。

## 语言 Cookie

WordPress 允许您在登录时更改所有可翻译字符串的语言。为此，WordPress 会设置一个名为 `wp_lang` 的 Cookie，这是一个会话 Cookie，用于存储所选语言的语言键。

## References

- [Wikipedia: Cookies](https://en.wikipedia.org/wiki/HTTP_cookie)
- [RFC2965](http://www.faqs.org/rfcs/rfc2965.html)
- [PHP cookie documentation](https://www.php.net/manual/en/features.cookies.php)