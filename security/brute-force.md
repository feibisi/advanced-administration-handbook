---
title: "暴力破解攻击"
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

# 暴力破解攻击

**暴力破解攻击**是最简单的入侵方式：攻击者反复尝试用户名/密码组合，直到成功为止。由于这些尝试是自动化的且通常分布式进行（僵尸网络），即使不成功的攻击也可能用请求淹没您的网站。

这并非 WordPress 独有——任何暴露登录界面的 Web 应用程序都可能成为目标——但 WordPress 的流行使其成为常见焦点。以下指南反映了 2025 年 WordPress 网站的最佳实践，并取代了旧有的、更脆弱的技术。

---

## 关键防护措施一览 {#key-defenses}
- **使用高强度、唯一的密码并配合密码管理器。**
- **为所有管理员账户启用双因素认证 (2FA)**（使用插件或您的身份提供商；WordPress 核心不包含 2FA）。
- **考虑通过信誉良好的插件使用通行密钥 (WebAuthn)**，以实现防钓鱼登录。
- **在边缘（WAF/CDN）或 Web 服务器上对登录尝试进行速率限制。**
- **在登录页面添加验证码/验证小部件** 以减缓机器人攻击（例如 Cloudflare Turnstile、reCAPTCHA）。
- **如果不需要，请保护或禁用 XML-RPC**；否则对其进行限制和速率限制。
- **保持 WordPress 核心、主题和插件为最新版本。**
- **监控并告警认证异常**；临时封禁恶意 IP。
- 优先使用**边缘/WAF 防护**（Cloudflare、Sucuri、主机提供商提供的 WAF），以便在恶意流量到达服务器前将其拦截。

> 提示：隐藏登录 URL 可以减少干扰，但不应作为唯一的防护手段。

---

## WordPress 层级防护 {#wordpress-level}

### 强制使用强密码 {#strong-passwords}
WordPress 在更改密码时会显示强度指示器。鼓励使用独特、冗长的密码（或密码短语）以及密码管理器。避免使用字典词汇和个人信息。

### 双重身份验证 (2FA) {#2fa}
为所有管理员和特权用户启用 2FA，使用信誉良好的插件或您的身份提供商（TOTP 应用、硬件密钥、短信备用）。截至 2025 年，**WordPress 核心不提供 2FA**；必须通过插件或 SSO/IdP 添加。

### 通行密钥 (WebAuthn) {#passkeys}
通行密钥通过平台验证器（Face ID/Touch ID、Windows Hello、安全密钥）提供防钓鱼的无密码登录。通过支持 WebAuthn/通行密钥的维护插件添加，并为每位管理员至少注册两个验证器。

### 应用程序密码（用于集成） {#application-passwords}
对于受信任应用/服务的 API 访问，请使用**应用程序密码**（WordPress 5.6 中引入）。它们限定访问范围，并且无需更改用户密码即可撤销。

### 限制登录尝试次数（应用层） {#limit-login}
如果您的托管/CDN服务未在边缘进行速率限制，安全插件可以限制登录尝试。请注意，应用层插件仍在PHP环境中执行，因此在遭受猛烈攻击时仍会消耗一些资源；如有可能，应优先选择边缘或服务器级限制。

### XML‑RPC 注意事项 {#xmlrpc}
`xmlrpc.php` 是常见的暴力攻击目标（尤其是 `system.multicall` 方法）。如果您不使用 XML‑RPC，请禁用它。如果使用（例如 Jetpack、移动应用），请限制其访问（通过 WAF 规则）并实施严格的速率限制。

## 服务器 / 代理 / WAF 防护 {#server-proxy}
> 这些示例需要服务器或代理访问权限，并且可能因环境而异。在应用到生产环境前，请在预演环境中测试。

### Apache (示例) {#apache}
**阻止或限制滥用登录尝试的速率**（示例需要适当的模块，如 `mod_rewrite`、`mod_authz_host`，或第三方工具如 ModSecurity 或 mod_evasive）。

**按 IP 拒绝 (Apache 2.4+):**
```apache
# wp-login.php: 仅允许特定 IP
<Files "wp-login.php">
    Require ip 203.0.113.15 203.0.113.16
</Files>
```

**将 401/403 错误发送到静态错误页面:**
```apache
ErrorDocument 401 /401.html
ErrorDocument 403 /403.html
```

> 考虑使用 ModSecurity 规则集（例如 OWASP CRS）在服务器层检测和阻止暴力破解模式。

### Nginx (examples) {#nginx}
**Rate‑limit login and XML‑RPC:**
```nginx
# Define a shared zone for rate limiting
limit_req_zone $binary_remote_addr zone=logins:10m rate=10r/m;

server {
    # ...

    location = /wp-login.php {
        limit_req zone=logins burst=20 nodelay;
        include fastcgi_params;
        # pass to PHP-FPM or upstream as usual
    }

    location = /xmlrpc.php {
        limit_req zone=logins burst=20 nodelay;
        include fastcgi_params;
        # pass to PHP-FPM or upstream as usual
    }
}
```

**Deny by IP:**
```nginx
location = /wp-login.php {
    allow 203.0.113.15;
    allow 203.0.113.16;
    deny all;
    # pass to PHP-FPM or upstream as usual
}
```

**Custom error pages:**
```nginx
error_page 401 /401.html;
error_page 403 /403.html;
```

### Caddy (v2) (examples) {#caddy}
**Password‑protect `/wp-login.php` with Basic Auth:**
```caddyfile
# Hash passwords first: `caddy hash-password`
basicauth /wp-login.php {
    user1 JDJhJDEw$example-hash-value...
    # add more users as needed (one per line)
}
```
> Caddy requires **hashed** passwords in the Caddyfile.

**Limit access to `/wp-login.php` by IP:**
```caddyfile
@blacklist {
    not client_ip forwarded 203.0.113.15 203.0.113.16
    path /wp-login.php
}
respond @blacklist "Forbidden" 403 {
    close
}
```

**Return 401 for `/wp-admin/*` and serve a custom error page:**
```caddyfile
@wpadmin path /wp-admin/*
respond @wpadmin "Unauthorized" 401

handle_errors {
    @need401 status 401
    rewrite @need401 /401.html
    file_server
}
```

**Deny “no‑referrer” POSTs to login/comments (optional, use with caution):**
```caddyfile
# Legitimate clients or privacy tools may omit Referer; test before enforcing.
@protected path_regexp protected (wp-comments-post|wp-login)\.php$
@no_referer {
    not header Referer https://{host}*
    method POST
}
abort @no_referer
```
> Using `abort` immediately drops the connection, which is efficient for bots.

For more Caddy discussion and rationale, see: *Using Caddy to deter brute force attacks in WordPress* (community thread).

### Windows IIS（示例）{#iis}
**使用 `web.config` 按 IP 限制 WP 管理后台：**
```xml
<location path="wp-admin">
  <system.webServer>
    <security>
      <ipSecurity allowUnlisted="false">
        <add ipAddress="203.0.113.15" allowed="true" />
        <add ipAddress="203.0.113.16" allowed="true" />
      </ipSecurity>
    </security>
  </system.webServer>
</location>
```

**自定义 401 错误页面：**
```xml
<system.webServer>
  <httpErrors errorMode="Custom">
    <remove statusCode="401" />
    <error statusCode="401" path="/401.html" responseMode="File" />
  </httpErrors>
</system.webServer>
```

---

## 主机/CDN WAF 防护 {#waf}
托管式 WAF（Cloudflare、Sucuri 或您的主机提供商）可以：
- 在网络边缘过滤已知的恶意 IP 和自动化登录尝试。
- 强制执行机器人管理、质询页面和特定于登录的规则。
- 对 `/wp-login.php` 和 `/xmlrpc.php` 全局应用**速率限制**。
- 为可疑请求添加 **CAPTCHA/turnstile** 质询。

> 优势：流量在到达您的服务器之前即被阻止，可在高流量攻击期间节省资源。

## 额外加固与操作建议 {#hardening}
- **不要使用用户名 `admin`。** 创建单独的管理员账户，并降级或移除旧用户。
- **限制管理员数量；** 日常操作使用最小权限角色。
- **审计日志**以记录失败的登录尝试并枚举来源；临时封禁恶意 IP（例如，使用 Fail2ban）。
- **避免使用永久性的国家封禁列表。** 它们可能误封合法用户且难以维护。
- **确保全程使用 HTTPS** 以保护传输中的凭据。
- **备份：** 维护经过测试、支持离线操作的备份，并演练恢复流程。

## 另请参阅 {#see-also}
- WordPress 高级管理：[安全](https://developer.wordpress.org/advanced-administration/security/)
- WordPress 高级管理：[强化 WordPress](https://developer.wordpress.org/advanced-administration/security/hardening/)
- WordPress.com：[暴力攻击防护](https://developer.wordpress.com/docs/platform-features/brute-force-attack-protection/)
- WordPress 核心：[应用密码（集成指南）](https://make.wordpress.org/core/2020/11/05/application-passwords-integration-guide/)
- 使用 Caddy 阻止 WordPress 中的暴力攻击 – 社区讨论：https://caddy.community/t/using-caddy-to-deter-brute-force-attacks-in-wordpress/13579

---

## 关于已弃用/遗留内容的说明 {#legacy-notes}
旧指南常建议使用繁重的 `.htaccess` 重写、国家 IP 黑名单或对整个 `/wp-admin` 目录启用 BasicAuth。如今这些措施要么不可靠，要么会破坏基于 AJAX 的插件，要么会降低用户体验。建议改用**边缘级 WAF**、**2FA**、**通行密钥**和**针对性速率限制**。若确实应用服务器级封锁，请将其范围限定在特定路径（如 `/wp-login.php`、`/xmlrpc.php`），并记录站点所需的例外情况（移动应用、Jetpack、SSO）。