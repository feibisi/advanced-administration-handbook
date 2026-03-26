---
title: "清空数据库表"
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

# 清空数据库表

为您生成网站统计数据的插件会快速创建大量数据——每位访客都会导致某些内容被写入数据库。通常这不是问题，但如果您的数据库大小受主机限制，则可能成为问题。此外，如果您因任何原因需要迁移数据库，其大小会影响导出和导入时间。本页将向您展示如何清空表，从而将其内容和大小重置为零。这不会阻止统计插件工作，也不会损坏您的数据库。

[phpMyAdmin](https://developer.wordpress.org/advanced-administration/upgrade/phpmyadmin/) 是用于操作数据库的程序名称。一个好的托管套餐会包含此工具。[访问 phpMyAdmin](https://developer.wordpress.org/advanced-administration/security/backup/#accessing-phpmyadmin) 提供了在各种服务器控制面板下访问 phpMyAdmin 的信息。

本文概述的流程已在运行于 Unix 上的 phpMyAdmin 版本 2.5.3 和 2.5.7 pl1 中经过尝试和测试。

**注意：** 在对数据库进行此类重大更改时，您应始终创建备份！

详情请参阅 [WordPress 备份](https://developer.wordpress.org/advanced-administration/security/backup/) 和 [备份数据库](https://developer.wordpress.org/advanced-administration/security/backup/database/)。

## 操作流程

1. 登录服务器上的 phpMyAdmin。

2. 从左侧边栏中选择您的 WordPress 数据库。

![左侧边栏中的数据库选择](https://user-images.githubusercontent.com/90067869/189547314-a8bbe78e-70b6-4533-b14e-196a5db35840.png)

3. WordPress 数据库中的所有表将显示出来。

![表列表](https://user-images.githubusercontent.com/90067869/189547350-944a1066-e81d-404b-8eca-9125161eb5d4.png)

4. 点击您希望清空的表的“清空”按钮。在本示例中，我们将清空“wp_dstats2”表。

**注意：** 您的表名可能不同，请查阅插件文档以确定其名称。请勿清空 WordPress 核心使用的表。（有关这些特定表名，请参阅 [Database_Description](https://codex.wordpress.org/Database_Description) 下的列表。）

![点击“清空”按钮](https://user-images.githubusercontent.com/90067869/189547374-2088ff00-3c19-420d-86b7-fbcd0df6ed6d.png)

7. 现在您将看到一个确认屏幕。

![图片](https://user-images.githubusercontent.com/90067869/189547394-d6a58758-7a2d-420c-9cd6-33de864b3078.png)

**这是您最后一次检查是否选择了正确的表和数据库的机会**——phpMyAdmin 没有撤销功能，因此一旦提交更改，您将无法回退。当然，除非您已按照建议进行了备份。

8. 点击“确定”，您将返回查看数据库中的所有表，指定表的内容已被清空。