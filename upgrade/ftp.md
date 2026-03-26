---
title: "使用 FTP 更新 WordPress"
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

# 使用 FTP 更新 WordPress

## FTP 客户端
有两种常见方式可在您的网站上上传和管理文件：

1. 使用主机控制面板提供的文件管理器。常用文件管理器：[cPanel](https://documentation.cpanel.net/display/64Docs/File+Manager)、[DirectAdmin](https://www.site-helper.com/filemanager.html)、[Plesk](https://www.plesk.com/)。
2. 使用 FTP 或 SFTP 客户端。本指南将展示如何使用 [FileZilla](https://filezilla-project.org/)。

两种选项并无绝对的“优劣”之分——它们只是操作服务器上同类文件的不同界面。

- **适合使用控制面板文件管理器的场景**：在浏览器中快速执行一次性任务（重命名文件、上传单个文件、查看服务器内容，或在无法安装软件时进行紧急小修改）。
- **适合使用 FTP/SFTP 客户端的场景**：较大或可重复的任务（上传多个文件/文件夹、手动更新时替换 WordPress 核心文件、上传主题/插件、保持队列运行、恢复中断的传输，或通过拖放和批量操作提高工作效率）。

### cPanel 是否比 FTP 客户端“更好”？
通常不是。cPanel 的文件管理器很方便，因为它可以在浏览器中运行，但对于批量上传来说可能更慢且更繁琐。FTP/SFTP 客户端在移动大量文件和文件夹时通常更高效。

### FTP/SFTP 客户端能“自动化”文件管理器中笨拙的部分吗？
是的，它们通常可以。客户端通常提供传输队列、递归文件夹上传、覆盖提示、可恢复传输和更便捷的批量操作等功能。底层任务相同（在服务器间复制文件），但工具使用起来会更顺畅。

### 两种方式上传的内容有区别吗？
通常两者都能上传/下载相同的网站文件。差异通常在于**限制和功能**，而非能力：

- **控制面板限制**：部分主机在浏览器中限制了上传大小、超时时间或 ZIP 解压功能。
- **FTP/SFTP 客户端的优势**：能更好地处理大量小文件，更轻松地进行文件夹镜像，以及在慢速连接上实现更可靠的传输。

FTP（文件传输协议）三十多年来一直是使用最广泛的传输协议，但它以明文发送信息，存在安全风险。如果主机支持，请使用 SFTP（安全文件传输协议）。它通过安全连接传输文件和密码，因此应尽可能替代 FTP。有时您需要联系主机为账户启用 SFTP。

为何选择 FileZilla？因为它与 WordPress 一样，基于 GPL 协议发布。所以它不仅免费，而且将始终保持免费。以下页面将展示如何设置和使用 FileZilla：

1. [为您的网站设置 FileZilla](https://developer.wordpress.org/advanced-administration/upgrade/ftp/filezilla/)
2. [设置权限](https://developer.wordpress.org/advanced-administration/server/file-permissions/)
3. [FileZilla 的详细文档](https://wiki.filezilla-project.org/Documentation)

想尝试其他 FTP 或 SFTP 客户端？[在维基百科上查看更多](https://en.wikipedia.org/wiki/Comparison_of_FTP_clients)。