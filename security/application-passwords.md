---
title: "应用密码"
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

# 应用密码

应用密码是 WordPress 的一项功能，允许您为程序化访问（例如移动应用、集成或脚本）生成**可撤销的、按应用分配的凭证**。其设计目的是避免与第三方工具共享您的主账户密码。

应用密码于 **WordPress 5.6**（2020 年 12 月）引入。请参阅：[应用密码：集成指南](https://make.wordpress.org/core/2020/11/05/application-passwords-integration-guide/)。

### 什么是应用密码？ {#what-are-application-passwords}

应用密码是：

*   与特定 WordPress 用户账户绑定的密码。
*   用于 API 身份验证（例如 WordPress REST API 和（如果启用）XML-RPC），而非用于交互式浏览器登录。
*   在 WordPress 中以哈希值存储，**仅在创建时显示一次**。
*   可单独撤销，因此您可以在不更改用户主密码的情况下禁用单个集成。

应用密码**不能用于通过 `wp-login.php` 登录 wp-admin**。它们适用于应用程序和脚本，而非用户在登录表单中手动输入凭据。

### 何时使用应用程序密码 {#when-to-use-application-passwords}

当你需要一个工具以 WordPress 用户身份进行身份验证，但又不想提供该用户的主密码时，请使用应用程序密码，例如：

* 需要调用 REST API 的部署/维护脚本。
* 向 WordPress 发布内容或上传媒体的第三方服务。
* 需要经过身份验证才能访问你网站 API 的桌面/移动客户端。

如果使用场景是浏览器中的交互式登录，请使用强密码并考虑启用**两步验证**（也称为双因素身份验证或 2FA）：[两步验证](https://developer.wordpress.org/advanced-administration/security/mfa/)。

### 在 wp-admin 中创建应用密码 {#creating-an-application-password-in-wp-admin}

1.  以需要凭据的用户身份（或以管理员身份编辑该用户）登录 wp-admin。
2.  转到 **用户 → 个人资料**（或 **用户 → 所有用户 → 编辑** 以编辑其他用户）。
3.  找到 **应用密码** 部分。
4.  输入一个描述性名称（例如：`CI 部署机器人`、`我的 iPhone 应用`、`报告集成`）。
5.  生成密码并 **立即复制/保存**（之后将不再显示）。

WordPress 将应用密码分组显示以提高可读性。使用时可以带空格或不带空格（验证前会去除空格）。

### 使用应用密码 {#using-an-application-password}

应用密码通常通过 HTTP 基本认证（[RFC 7617](https://datatracker.ietf.org/doc/html/rfc7617)）使用。使用基本认证时，客户端会在 `Authorization` HTTP 头部中发送凭据。

该头部值是通过对字符串 `username:password` 进行 Base64 编码构建的：

```
Authorization: Basic BASE64_ENCODED_CREDENTIALS
```

使用应用密码时：

* **用户名**：WordPress 用户名（登录名）。
* **密码**：生成的应用密码。

由于基本认证凭据如果在未加密的情况下通过网络发送可能会被截获，因此应始终使用 HTTPS。参见：[HTTPS](https://developer.wordpress.org/advanced-administration/security/https/)。

示例（使用 `curl` 的 REST API 请求）：

```
curl --user "USERNAME:APPLICATION_PASSWORD" https://example.com/wp-json/wp/v2/users/me
```

### 管理与轮换应用密码 {#managing-and-rotating-application-passwords}

在用户个人资料的同一**应用密码**区域，您可以：

* 查看现有应用密码（按名称）。
* 查看使用元数据（例如，最后使用时间 / 最后 IP）。
* 撤销单个密码。

操作最佳实践是将应用密码视为机密信息：

* 为每个集成/应用创建一个（不要在工具间重复使用）。
* 撤销任何不再需要的凭据。
* 定期或在任何疑似泄露后轮换凭据。

### 使用 WP-CLI 管理应用程序密码 {#managing-application-passwords-with-wp-cli}

如果您通过 SSH 管理 WordPress，WP-CLI 可以创建、列出、更新和撤销应用程序密码。

示例：

```
# 为用户 ID 123 创建一个应用程序密码（密码仅显示一次）。
wp user application-password create 123 "myapp"

# 为用户 ID 123 创建并仅输出密码（适用于脚本/CI）。
wp user application-password create 123 "myapp" --porcelain

# 列出用户 ID 123 的密码并显示常用字段。
wp user application-password list 123 --fields=uuid,name,created,last_used,last_ip

# 通过 UUID 删除用户 ID 123 的特定应用程序密码。
wp user application-password delete 123 <uuid>
```

官方命令参考：[wp user application-password](https://developer.wordpress.org/cli/commands/user/application-password/)。

### 可用性与禁用 {#availability-and-disabling}

默认情况下，当请求通过 HTTPS 提供服务时，应用程序密码功能可用。其可用性可通过过滤器进行自定义（或禁用）。

若您不希望网站启用应用程序密码功能，可通过代码（例如在必备插件中）使用 `wp_is_application_passwords_available` 过滤器来禁用。如需更精细的控制（例如仅允许特定用户/角色使用），请使用 `wp_is_application_passwords_available_for_user` 过滤器。

### 故障排除 {#troubleshooting}

如果身份验证失败（401/403）或用户配置文件中未显示应用密码：

* **确认 HTTPS**：应用密码专用于 HTTPS 请求。请确保站点已正确配置 TLS 且 WordPress 能检测到 HTTPS。参见：[HTTPS](https://developer.wordpress.org/advanced-administration/security/https/)。
* **确认您正在使用应用密码**：交互式登录可能仍需要用户的常规密码；应用密码是独立的。
* **检查功能是否被禁用**：安全插件、必须使用插件或自定义代码可能通过过滤器禁用应用密码。
* **确认客户端发送基本认证**：某些 HTTP 客户端和代理可能会剥离 `Authorization` 标头，除非明确配置。

### 相关内容 {#related}

* [HTTPS](https://developer.wordpress.org/advanced-administration/security/https/)
* [两步验证](https://developer.wordpress.org/advanced-administration/security/mfa/)
* WP-CLI 参考：[wp user application-password](https://developer.wordpress.org/cli/commands/user/application-password/)