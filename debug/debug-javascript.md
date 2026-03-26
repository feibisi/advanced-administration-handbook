---
title: "使用浏览器诊断 JavaScript 错误"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Debug
    - Repos
    - Data
---

# 使用浏览器诊断 JavaScript 错误

如果您遇到交互功能问题，可能是由于 JavaScript 错误或冲突导致的。例如，下拉菜单可能失效、元框无法拖动或添加媒体按钮无法工作。为了有效提交支持请求，让团队了解具体的 JavaScript 错误信息将大有帮助。

本指南将向您展示如何在不同的浏览器中诊断 JavaScript 问题。

## 步骤 1：尝试其他浏览器

不同浏览器对 JavaScript 的实现方式常有差异。为确保这是 JavaScript 错误而非浏览器错误，请首先尝试在其他浏览器中打开您的网站。

* 若网站在新浏览器中没有相同问题，则说明错误是特定于浏览器的
* 若网站出现相同错误，则说明该错误并非特定于单一浏览器

请记录所有出现此错误的浏览器信息。在提交支持请求时，这些信息将非常有用。

## 步骤 2：启用 SCRIPT_DEBUG

你需要开启[脚本调试](https://developer.wordpress.org/advanced-administration/debug/debug-wordpress/#script_debug)。打开 `wp-config.php` 文件，在 "That's all, stop editing! Happy blogging" 这一行之前添加以下代码：

```
define('SCRIPT_DEBUG', true);
```

检查问题是否仍然存在。

* **问题已解决** – 关闭脚本调试，并在支持论坛报告此问题，告知志愿者你开启了脚本调试并解决了问题。
* **问题仍然存在** – 继续执行步骤 3。

## 步骤 3：诊断

### 打开开发者工具

*   **Chrome**：输入 `Cmd-Option-J` (Mac) 或 `Ctrl-Shift-J` (Windows, Linux, Chrome OS)，或在菜单中导航至 `查看 -> 开发者 -> 开发者工具`。
*   **Firefox**：输入 `Cmd-Option-K` (Mac) 或 `Ctrl-Shift-K` (Windows, Linux, Chrome OS)，或在菜单中导航至 `Web 开发 -> Web 控制台`。
*   **Edge**：遵循 Chrome 的说明。
*   **Safari**：首先，导航至 `Safari -> 偏好设置`。点击 `高级` 标签页，然后勾选 `在菜单栏中显示“开发”菜单`。接着，在新的 `开发` 菜单中，导航至 `显示 JavaScript 控制台`。
*   **Opera**：在菜单中导航至 `工具 -> 高级 -> 错误控制台`。

### 识别错误

通常，错误信息会直接显示在控制台中。它可能看起来类似这样：

![开发者工具错误页面示例截图。](https://wordpress.org/documentation/files/2020/07/chrome-devtools.png)

上图显示错误位于 `jquery.js 的第 2 行`，但请记住要复制完整的堆栈信息！仅说明行号不如展示上下文更有帮助。

如果没有显示错误，请重新加载页面；许多错误仅在页面首次加载时出现。

## Step 4: Reporting

Now that you have diagnosed your error, you should make your support forum request. Go to the [troubleshooting forum](https://wordpress.org/documentation/forum/how-to-and-troubleshooting).

If your problem is with a specific theme or plugin, you can access their dedicated support forum by visiting https://wordpress.org/support/plugin/PLUGINNAME or https://wordpress.org/support/theme/THEMENAME.

Please include the below information:

* the browsers that you are experiencing the problem in
* whether `SCRIPT_DEBUG` fixed the error or not
* the JavaScript error
* the location of the error – both the file name and the line number
* the context of the error – including the whole error stack will help developers
* If possible, a link to the web page showing the error