---
title: "WordPress 调试"
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

# WordPress 调试

调试 PHP 代码是任何项目的一部分，但 WordPress 配备了特定的调试系统，旨在简化调试过程并标准化核心、插件和主题中的代码。本页介绍了 WordPress 中可用的各种调试工具，以及如何提高编码效率，同时提升代码的整体质量和互操作性。

对于非程序员或普通用户，这些选项可用于显示错误的详细信息。

**注意**：在对网站进行任何修改之前，请确保您已使用暂存环境或对网站进行了适当的备份。

## 调试用 wp-config.php 示例

将以下代码插入您的 [wp-config.php](https://wordpress.org/documentation/article/editing-wp-config-php/) 文件，会将所有错误、通知和警告记录到 `wp-content` 目录下名为 `debug.log` 的文件中。它还会隐藏错误，使其不会中断页面生成。

```
// 启用 WP_DEBUG 模式
define( 'WP_DEBUG', true );
```

```
// 启用调试日志记录到 /wp-content/debug.log 文件
define( 'WP_DEBUG_LOG', true );
```

```
// 禁用错误和警告的显示
define( 'WP_DEBUG_DISPLAY', false );
@ini_set( 'display_errors', 0 );
```

```
// 使用核心 JS 和 CSS 文件的开发版本（仅在修改这些核心文件时需要）
define( 'SCRIPT_DEBUG', true );
```

**注意**：您必须将此代码插入到 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件中的 `/* 就这些，停止编辑！祝您博客愉快。 */` **之前**。

## WP_DEBUG

`WP_DEBUG` 是一个 PHP 常量（永久全局变量），可用于在整个 WordPress 中触发“调试”模式。默认情况下它被假定为 false，通常在 WordPress 开发副本的 [wp-config.php](https://developer.wordpress.org/advanced-administration/wordpress/wp-config/) 文件中设置为 true。

```
// 启用调试模式。
define( 'WP_DEBUG', true );
```

```
// 禁用调试模式。
define( 'WP_DEBUG', false );
```

**注意**：示例中的 `true` 和 `false` 值没有用单引号 (') 包围，因为它们是布尔值（真/假）。如果将常量设置为 `'false'`，它将被解释为 true，因为引号使其成为字符串而非布尔值。

不建议在正式站点上使用 `WP_DEBUG` 或其他调试工具；它们仅用于本地测试和暂存安装。

### PHP 错误、警告与通知

启用 `WP_DEBUG` 将显示所有 PHP 错误、通知和警告。这可能会改变 PHP 的默认行为——PHP 通常仅显示致命错误，或在遇到错误时呈现白屏死机。

显示所有 PHP 通知和警告常会暴露看似未损坏但未遵循 PHP 内部数据验证规范的代码问题。一旦识别出相关代码，这些警告很容易修复，且修复后的代码几乎总能增强抗错能力并提升可维护性。

### 自定义 PHP 调试

若出于调试目的需要记录非错误信息，PHP 确实提供了 `error_log` 函数。然而，此方法默认不提供格式正确的输出。

为解决此问题，您可以在站点上添加另一个函数来处理格式化，可以通过创建[自定义插件](https://developer.wordpress.org/plugins/plugin-basics/)或使用带有[代码片段](https://wordpress.org/plugins/search/code+snippets/)插件的代码片段来实现。该函数将作为 `error_log` 的包装器，在记录前使用 `print_r` 正确格式化数组和对象。

以下是一个示例函数，它要求启用 `WP_DEBUG`。

```
function write_log( $data ) {
	if ( true === WP_DEBUG ) {
		if ( is_array( $data ) || is_object( $data ) ) {
			error_log( print_r( $data, true ) );
		} else {
			error_log( $data );
		}
	}
}
```

使用示例：

```
write_log( 'DEBUG TEXT' );
write_log( $variable );
```

**注意**：不建议在 `functions.php` 中添加如上例的自定义代码，以避免维护、安全、性能、兼容性和代码组织问题。

### 已弃用的函数与参数

启用 `WP_DEBUG` 还会显示您网站上正在使用的 WordPress 中已弃用的函数和参数的相关通知。这些是尚未从核心代码中移除，但计划在近期删除的函数或函数参数。弃用通知通常会指明应使用的新函数。

## WP_DEBUG_LOG

`WP_DEBUG_LOG` 是 `WP_DEBUG` 的配套功能，它会导致所有错误同时被保存到 `debug.log` 日志文件中。如果您想稍后查看所有通知，或者需要查看在屏幕外生成的通知（例如，在 AJAX 请求或 `wp-cron` 运行期间），这将非常有用。

请注意，这允许您使用 PHP 内置的 `error_log()` 函数写入日志文件，这在调试 Ajax 事件等情况下非常有用。

当设置为 `true` 时，日志将保存在您站点文件系统的内容目录下的 `debug.log` 文件中（通常是 `wp-content/debug.log`）。或者，您可以将其设置为一个有效的文件路径，以便将文件保存在其他位置。

```
define( 'WP_DEBUG_LOG', true );
```

-或-

```
define( 'WP_DEBUG_LOG', '/tmp/wp-errors.log' );
```

**注意**：要使 `WP_DEBUG_LOG` 生效，必须启用 `WP_DEBUG`（设为 true）。请记住，您可以独立地关闭 `WP_DEBUG_DISPLAY`。

## WP_DEBUG_DISPLAY

`WP_DEBUG_DISPLAY` 是 `WP_DEBUG` 的另一个辅助常量，用于控制是否在页面的 HTML 中显示调试信息。默认值为 'true'，即在生成时显示错误和警告。将其设置为 false 将隐藏所有错误。这应与 `WP_DEBUG_LOG` 配合使用，以便稍后可以查看错误。

```
define( 'WP_DEBUG_DISPLAY', false );
```

**注意**：要使 `WP_DEBUG_DISPLAY` 生效，必须启用 `WP_DEBUG`（设为 true）。请记住，您可以独立控制 `WP_DEBUG_LOG`。

## SCRIPT_DEBUG

`SCRIPT_DEBUG` 是一个相关常量，它会强制 WordPress 使用核心 CSS 和 JavaScript 文件的“开发”版本，而不是通常加载的压缩版本。这在测试对任何内置 `.js` 或 `.css` 文件的修改时非常有用。默认值为 `false`。

```
define( 'SCRIPT_DEBUG', true );
```

## SAVEQUERIES

`SAVEQUERIES` 定义将数据库查询保存到数组中，随后可以显示这些查询以帮助进行分析。当该常量设置为 true 时，会导致每个查询与其执行时间及调用它的函数一同被保存。

```
define( 'SAVEQUERIES', true );
```

该数组存储在全局变量 `$wpdb->queries` 中。

**注意**：这会对您的网站性能产生影响，因此请确保在非调试时关闭此功能。

## 调试插件

WordPress 有许多[调试插件](https://wordpress.org/plugins/search/debug/)，用于显示内部运行的更多信息，无论是针对特定组件还是通用信息。

例如，[Debug Bar](https://wordpress.org/plugins/debug-bar/)会在管理栏添加一个调试菜单，显示查询、缓存和其他有用的调试信息。当启用 WP_DEBUG 时，它还会追踪 PHP 警告和通知，使其更易于查找。