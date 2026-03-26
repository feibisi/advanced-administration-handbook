---
title: "使用 FileZilla"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Upgrade
    - Repos
    - Data
---

# 使用 FileZilla

[FileZilla](https://filezilla-project.org/) 是一款开源的文件传输客户端和服务器。客户端和服务器均适用于 Windows、Linux 和 macOS。FileZilla 支持 **SFTP** 和 **FTPS**（加密）以及其他协议。如果您的托管服务支持，请优先使用 **SFTP** 而非普通 FTP。

您可以使用 FileZilla 通过上传和下载文件及图片来管理您的 WordPress 站点。本文将指导您安装和使用 FileZilla FTP 客户端来管理您的 WordPress 站点。

![FileZilla 应用程序截图](https://raw.githubusercontent.com/WordPress/Advanced-administration-handbook/main/assets/filezilla_1.gif)

有关 FileZilla 的更多信息，请查看官方网站的[功能列表](https://filezilla-project.org/client_features.php)。

### 为何要下载 FileZilla？

它快速、稳定、易用且免费。FTP 是在本地系统与网络服务器之间上传或下载文件的标准方式，而 FileZilla 是满足日常 FTP 需求的可靠客户端。

## 设置选项

您需要从主机获取以下详细信息（适用于 FTP、FTPS 或 SFTP）：

1.  **主机/服务器名称**（通常是类似 `example.com`、`ftp.example.com` 或主机提供的服务器名称）
2.  **协议**（如果可用，**首选 SFTP**；有些主机提供 **FTPS**）
3.  您的**用户名**
4.  您的**密码**（或者，对于 SFTP，有时是 SSH 密钥）

如果您的服务器上还没有 FTP 账户，请使用控制面板或网站管理工具创建一个——它将包含所有必需的信息。如有疑问，请向您的主机咨询有关用于访问您网站空间的 FTP 账户的说明或帮助。

在连接 FTP 服务器之前，您应该在站点管理器中注册它。一旦注册，只需单击一下即可连接到同一服务器。

要注册 FTP 服务器，请按照以下步骤操作：

1.  在 FileZilla 主窗口中，点击 **文件 → 站点管理器**。
2.  点击 **新站点**，然后为您的新连接命名（例如：我的博客服务器）。
3.  在 **主机** 框中输入主机/服务器名称。这可能是 `ftp.example.com`、`example.com` 或主机提供的值。如果您不知道，请向您的主机核实。注意：除非您的主机特别指示，否则不要在末尾添加 `/`。
4.  为您的账户选择正确的 **协议**（可用时**首选 SFTP**）。除非您的主机提供特定端口，否则将 **端口** 留空。常见的默认端口是 22（SFTP）和 21（FTP）。
5.  在登录类型框中选择 **普通**
6.  在用户框中输入您获得的完整用户名。它可能只是一个用户名，或者看起来像电子邮件地址（但实际上不是）。例如，它可能类似于 `user` 或 `user@example.com`。
7.  输入密码。请注意密码可能区分大小写。
8.  点击确定。

![FileZilla 站点管理器窗口的屏幕截图。](https://raw.githubusercontent.com/WordPress/Advanced-administration-handbook/main/assets/filezilla_3.gif)

## 连接

前往 FileZilla 主窗口工具栏的第一个图标"打开站点管理器"，点击下拉箭头，从下拉列表中选择您的 FTP 服务器。

或者，通过 **文件 → 站点管理器** 启动站点管理器，选择您的 FTP 服务器，然后点击"连接"。

在小窗口（消息日志）中出现一系列消息后，您应该会在大窗口（远程目录树）中看到服务器上的文件列表。

## 故障排除

如果您遇到问题，那么是时候开始故障排除了！

查看 FileZilla 主窗口的顶部区域并检查消息。

1. 如果没有任何连接尝试，那么服务器/主机名是错误的（或者协议/端口不正确）。只要有一个字符错误就会失败。点击红色 X 断开连接，然后点击站点管理器检查您输入的内容。
2. 如果显示用户不存在或 _登录错误_ 等，请检查站点管理器设置，确保其与您的托管服务商提供的 FTP 账户和密码信息一致，或者使用托管服务商提供的 Web 服务器管理界面重新检查 FTP 账户是否存在。仔细检查您的密码，它是区分大小写的（大写和小写字母）。您可能还需要向您的网站托管服务商寻求一些帮助。
3. 如果显示 _无法检索目录列表_，您可能需要更改传输设置。在站点管理器中，选择您的 FTP 服务器并点击 *传输设置* 选项卡。从传输模式中选择 *被动* 并点击确定。