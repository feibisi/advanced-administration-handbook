---
title: "缓存安全"
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

# 缓存安全

虽然缓存能显著提升 WordPress 网站性能，但如果缓存提供程序配置不当，也可能使网站暴露于新的安全风险。常见漏洞包括但不限于：网站访问其他网站的缓存数据，或缓存应用提供错误的缓存数据/文件。各类缓存应用通常都设有安全设置与配置选项，以确保在享受缓存性能优势的同时提供安全环境。

## OpCache 安全

如 WordPress 托管手册的性能章节所述，PHP 操作码缓存能显著提升 WordPress 网站的 PHP 处理性能。然而，若配置不当，PHP 操作码缓存可能允许用户未经授权访问其他用户的 PHP 文件。存在重要的 PHP 配置选项可缓解此类漏洞，例如未经授权访问文件。

### 验证权限

以下设置使 PHP 检查当前用户是否具有访问缓存文件的必要权限。应在根 php.ini 配置级别启用此设置，以防止用户访问其他用户的缓存文件。  
`opcache.validate_permission = On`

此设置默认未启用。且仅在 PHP 7.0.14 及更高版本中可用。

### 验证根目录

以下设置可防止 PHP 用户访问其通常无法访问的 chroot 目录之外的文件。此设置也应添加到根 php.ini 配置层级，以防止对文件的未授权访问。  
`opcache.validate_root = On`

此设置默认未启用。且仅在 PHP 7.0.14 及更高版本中可用。

### 限制 API 访问权限

通常情况下，任何 PHP 用户都可以访问 opcache API 来查看当前缓存的文件并管理 PHP 操作码缓存。然而，在某些 PHP 配置中，PHP 操作码缓存在服务器上的所有用户之间共享内存。这意味着所有用户都可以查看和访问 PHP 操作码缓存，并能访问其他用户缓存的 PHP 文件。限制 Opcache API 可以防止在未授权目录中运行的 PHP 脚本查看缓存文件，以及从 PHP 脚本内部手动与 PHP 操作码缓存交互。以下设置定义了 PHP 脚本必须从哪个目录路径开始才能访问 Opcache API：  
`opcache.restrict_api = '/some/folder/path'`

该设置的默认值为 `''`，这意味着对哪些 PHP 脚本可以访问 Opcache API 没有限制。此设置应在 PHP 配置的根 php.ini 文件中定义，以防止用户覆盖它。

## 对象缓存安全

WordPress 提供多种数据库对象缓存解决方案。每种方案都有其特定的配置要求，以确保在使用数据库对象缓存时提供安全的环境。

### Redis

Redis 是一款轻量级、高性能的键值数据库服务器，常用于缓存 WordPress 数据库查询结果。在默认配置下，Redis 使用单一数据库，无需用户名和密码即可访问。同时，Redis 应仅允许来自授权网络主机的访问。

#### Redis 数据库

Redis 默认提供 16 个数据库，编号从 0 到 15。Redis 客户端应配置为使用不同的数据库，而非默认的 0 号数据库。Redis 可配置为拥有更多数据库，但这超出了本文档的范围。

#### Redis 用户凭证

若 Redis 将用于数据库对象缓存，则应配置 Redis 服务器要求访问凭证。

#### Redis 网络主机

默认配置下，Redis 服务器监听 6379 端口。该端口可在 Redis 配置中修改，但无论使用哪个端口，都应通过防火墙进行保护以防止未授权访问。

#### Redis 缓存键盐值

若使用 Redis 进行数据库对象缓存，采用唯一的 Redis 缓存键盐值有助于防止缓存冲突——即当两个网站尝试使用相同键名缓存内容时发生的情况。缓存冲突可能导致网站访问其他网站的缓存数据，并引发其他非预期的不良行为。Redis 缓存键盐值通常通过 Redis 缓存插件或 Redis 客户端进行配置，这些工具用于在 WordPress 网站中启用 Redis 数据库对象缓存功能。

### Memcached

Memcached 是一种内存对象缓存解决方案，常用于为 WordPress 提供数据库对象缓存。使用 Memcached 时最重要的配置注意事项之一是防止 Memcached 被公共互联网访问。将 Memcached 服务器置于防火墙之后，是安全使用 Memcached 进行 WordPress 数据库对象缓存的最重要环节之一。