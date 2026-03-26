---
title: "WordPress 多站点 / 网络"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Multisite
    - Repos
    - Data
---

# WordPress 多站点 / 网络

WordPress 多站点是 WordPress 的一项功能，允许您在一个安装中管理多个 WordPress 实例。使用多站点需要启用重写功能。详情请查看[服务器要求](https://developer.wordpress.org/advanced-administration/multisite/prepare-network/#server-requirements)。

多站点可用于多种目的。例如，商业网站常使用多站点来共享某些资源（如主题或插件），同时为不同地区提供不同的内容。

多站点中的内容在数据库中有其独立的表，只有用户表在各个实例间共享。

您可以创建使用子目录（“基于路径”）的多站点，也可以使用域名或子域名（“基于域名”）。关于如何映射域名，请参阅[WordPress 多站点域名映射](https://developer.wordpress.org/advanced-administration/multisite/domain-mapping/)。