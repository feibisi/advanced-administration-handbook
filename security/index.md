---
title: "安全"
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

# 安全
本页面的目标是向管理 WordPress 站点的用户介绍通用的安全最佳实践，涵盖环境层面（如文件权限）和应用层面（如设置适当的用户角色），以便他们在部署 WordPress 时获得比默认配置更扎实的安全基础。

**WordPress 安全最重要的事项是保持 WordPress 核心、所有已安装插件和主题的更新。同时鼓励用户选择持续接收更新的主题和插件。**

WordPress 致力于为用户提供安全的使用体验。关于 WordPress 官方安全立场及整体安全目标的详细讨论，请参阅 [WordPress.org 安全页面](https://wordpress.org/about/security/)。

本指南大量借鉴了 WordPress Codex 中的 [强化 WordPress](https://wordpress.org/support/article/hardening-wordpress/) 指南。由于该文档允许公开编辑，阅读时应谨慎参考其中建议。

保持任何系统（不仅是 WordPress）的安全是一项持续的工作。良好的安全需要周密的规划、监控和定期维护。

安全主要包含降低风险和制定恢复计划。多数安全方案仅聚焦于最小化未授权访问风险，但风险永远无法完全降为零。只要存在风险，就必须制定恢复计划，确保在发生意外时用户站点不会完全丢失，并能快速恢复正常运行。

安全不仅关乎 WordPress，还涉及确保托管环境的安全以及个人在线行为的安全性。良好的安全既取决于所用技术，也取决于使用技术的人员。过时技术可能存在的缺陷或漏洞会使 WordPress 网站面临风险。不良的在线操作习惯同样会危及网站安全。因此，不仅要保持所用技术的更新和维护，还需确保员工在使用互联网、操作托管平台或客户 WordPress 站点时遵循安全最佳实践。