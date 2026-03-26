---
title: "文章格式"
post_status: publish
comment_status: open
taxonomy:
  category:
    - advanced-administration-handbook
  post_tag:
    - Wordpress
    - Repos
    - Data
---

# 文章格式

## 简介

**文章格式**是随 [3.1 版本](https://wordpress.org/documentation/wordpress-version/version-3-1/)引入的一项[主题功能](https://codex.wordpress.org/Theme_Features)。文章格式是一种元信息，主题可利用它来自定义文章的呈现方式。文章格式功能提供了一个标准化的格式列表，所有支持此功能的主题均可使用。主题无需支持列表中的所有格式。新格式不能由主题甚至插件引入。该列表的标准化既保证了众多主题间的兼容性，也为外部博客工具提供了一致访问此功能的途径。

简而言之，使用支持文章格式的主题时，博主可通过单选按钮列表选择文章格式，从而改变每篇文章的显示效果。

以**日志**为例，过去需要创建一个名为“日志”的分类，将文章分配至该分类，然后基于 [post_class()](https://developer.wordpress.org/reference/functions/post_class/) 或 [in_category('asides')](https://developer.wordpress.org/reference/functions/in_category/) 的样式规则进行差异化显示。通过**文章格式**，新方法允许主题添加对文章格式的支持（例如 [add_theme_support('post-formats', array('aside'))](https://developer.wordpress.org/reference/functions/add_theme_support/)），随后在保存文章时即可通过发布元框选择文章格式。可通过调用 [get_post_format($post->ID)](https://developer.wordpress.org/reference/functions/get_post_format/) 函数确定格式，且 [post_class()](https://developer.wordpress.org/reference/functions/post_class/) 还会生成“format-asides”类，以便进行纯 CSS 样式设计。

## 支持的格式

如果主题启用了支持，用户可以选择以下文章格式。

请注意，虽然实际的文章内容输入不会改变，但主题可以根据用户选择的格式，使用此选择以不同方式显示文章。例如，主题可以不为“状态”文章显示标题。具体如何显示完全由主题决定，但以下是一些通用指南。

* **aside（日志）**：通常不显示标题。类似于 Facebook 的笔记更新。
* **gallery（相册）**：一个图片库。文章可能包含一个相册短代码，并附有图片附件。
* **link（链接）**：指向另一个网站的链接。主题可能希望使用文章内容中的第一个 `<a href="">` 标签作为该文章的外部链接。另一种方法是，如果文章仅包含一个 URL，则该 URL 将成为链接地址，而标题 (`post_title`) 将成为该链接锚点的名称。
* **image（图像）**：单张图片。文章中的第一个 `<img>` 标签可被视为图片。或者，如果文章仅包含一个 URL，则该 URL 将成为图片地址，文章标题 (`post_title`) 将成为图片的标题属性。
* **quote（引语）**：一段引文。可能包含一个包含引文内容的块引用。或者，引文可能只是内容，而来源/作者作为标题。
* **status（状态）**：简短的状态更新，类似于 Twitter 的状态更新。
* **video（视频）**：单个视频或视频播放列表。文章内容中的第一个 `<video>` 标签或 object/embed 可被视为视频。或者，如果文章仅包含一个 URL，则该 URL 将成为视频地址。如果博客启用了视频支持（例如通过插件），也可能包含作为文章附件的视频。
* **audio（音频）**：音频文件或播放列表。可用于播客。
* **chat（聊天）**：聊天记录，例如：
```
John: foo
Mary: bar
John: foo 2
```

注意：在撰写或编辑文章时，“标准”用于表示未指定文章格式。此外，如果指定的格式无效，则将使用标准（无格式）。

## 函数参考

**主要函数**：[set_post_format()](https://developer.wordpress.org/reference/functions/set_post_format/)、[get_post_format()](https://developer.wordpress.org/reference/functions/get_post_format/)、[has_post_format()](https://developer.wordpress.org/reference/functions/has_post_format/)。

**其他函数**：[get_post_format_link()](https://developer.wordpress.org/reference/functions/get_post_format_link/)、[get_post_format_string()](https://developer.wordpress.org/reference/functions/get_post_format_string/)。

## 添加主题支持

主题需要在 _functions.php_ 文件中使用 [add_theme_support()](https://developer.wordpress.org/reference/functions/add_theme_support/) 函数，通过传递格式数组来告知 WordPress 支持哪些文章格式，例如：

```
add_theme_support( 'post-formats', array( 'aside', 'gallery' ) );
```

请注意，必须在 [init](https://developer.wordpress.org/reference/hooks/init/) 钩子被调用之前执行此操作！推荐使用 [after_setup_theme](https://developer.wordpress.org/reference/hooks/after_setup_theme/) 钩子。

## 添加文章类型支持

文章类型需要在 _functions.php_ 文件中使用 [add_post_type_support()](https://developer.wordpress.org/reference/functions/add_post_type_support/) 来告知 WordPress 支持哪些文章格式：

```
// 为文章类型 'page' 添加文章格式支持
add_post_type_support( 'page', 'post-formats' );
```

下一个示例注册了自定义文章类型 `my_custom_post_type`，并添加了文章格式支持。

```
// 注册自定义文章类型 'my_custom_post_type'
add_action( 'init', 'create_my_post_type' );
function create_my_post_type() {
  register_post_type( 'my_custom_post_type',
    array(
      'labels' => array( 'name' => __( 'Products' ) ),
      'public' => true
    )
  );
}

// 为文章类型 'my_custom_post_type' 添加文章格式支持
add_post_type_support( 'my_custom_post_type', 'post-formats' );
```

或者，在函数 [register_post_type()](https://developer.wordpress.org/reference/functions/register_post_type/) 中，将 `post-formats` 添加到 `supports` 参数数组中。下一个示例与上一个等效。

```
// 使用 'supports' 参数注册自定义文章类型 'my_custom_post_type'
add_action( 'init', 'create_my_post_type' );
function create_my_post_type() {
  register_post_type( 'my_custom_post_type',
    array(
      'labels' => array( 'name' => __( 'Products' ) ),
      'public' => true,
      'supports' => array('title', 'editor', 'post-formats')
    )
  );
}
```

## 使用文章格式

在主题中，使用 [get_post_format()](https://developer.wordpress.org/reference/functions/get_post_format/) 来检查文章的格式，并相应地改变其呈现方式。请注意，使用默认格式的文章将返回 FALSE 值。或者使用 [has_post_format()](https://developer.wordpress.org/reference/functions/has_post_format/) [条件标签](https://codex.wordpress.org/Conditional_Tags)：

```
if ( has_post_format( 'video' )) {
  echo 'this is the video format';
}
```

另一种使用格式的方式是通过样式规则。主题应在包裹文章的包装代码中使用 [post_class()](https://developer.wordpress.org/reference/functions/post_class/) 函数来添加动态样式类。文章格式会以这种方式添加额外的类，使用 "format-foo" 这样的命名。

例如，可以通过在主题的样式表中添加以下代码，来隐藏状态格式文章的标题：

```
.format-status .post-title {
  display:none;
}
```

### 推荐样式

虽然你可以按需自定义格式的显示样式，但根据现代使用惯例，每种格式都有其适合的"风格"。牢记每种格式的预期用途很重要，这有助于读者直观地将其识别为特定类型的内容。

例如，旁注、链接和状态格式通常不显示标题或作者信息。它们简洁、短小且次要。旁注可能包含一两个段落，而链接可能只是一句带URL链接的话。链接和旁注可能包含指向单篇文章页面的链接（使用[the_permalink()](https://developer.wordpress.org/reference/functions/the_permalink/)）从而允许评论，但状态格式很可能没有此类链接。

另一方面，图片文章通常只包含单张图片，可配说明文字。音频/视频文章类似但包含音视频内容。这三种格式都可以使用插件或标准[嵌入功能](https://wordpress.org/documentation/article/embeds/)来显示内容。它们也可能不显示标题和作者信息，因为内容本身已能说明问题。

引用格式特别适合展示不带额外信息的简单引述。如果将引文放在文章内容中，将被引用者姓名设为文章标题，就可以将[the_content()](https://developer.wordpress.org/reference/functions/the_content/)重新设计为引用块样式单独显示，并用[the_title()](https://developer.wordpress.org/reference/functions/the_title/)将被引用者姓名显示为署名行。

聊天记录尤其适合等宽字体显示。通过对`.format-chat`添加样式，可以用等宽字体显示文章内容（例如置于灰色背景div中），从而在视觉上将其区分为聊天会话。

### 子主题中的文章格式

[子主题](https://developer.wordpress.org/themes/advanced-topics/child-themes/)会继承父主题定义的文章格式。在子主题中调用 [add_theme_support()](https://developer.wordpress.org/reference/functions/add_theme_support/) 来设置文章格式时，必须使用比父主题更高的优先级，并且会**覆盖**现有列表，而非追加到其中。

```
add_action( 'after_setup_theme', 'childtheme_formats', 11 );
function childtheme_formats() {
  add_theme_support( 'post-formats', array( 'aside', 'gallery', 'link' ) );
}
```

调用 [remove_theme_support('post-formats')](https://developer.wordpress.org/reference/functions/remove_theme_support/) 则会完全移除文章格式支持。

## 向后兼容性

如果您的插件或主题需要与早期版本的 WordPress 兼容，您需要向 `post_format` 分类法添加名为 `post-format-$format` 的术语。例如，

```
wp_insert_term( 'post-format-aside', 'post_format' );
```

您还必须使用 [register_taxonomy()](https://developer.wordpress.org/reference/functions/register_taxonomy/) 注册 `post_format` 分类法。

## 源文件

* [wp-includes/post-formats.php](https://core.trac.wordpress.org/browser/tags/4.4.2/src/wp-includes/post-formats.php#L0)

## 外部资源

* [使用 WordPress 自定义文章格式样式化聊天记录](https://www.narga.net/styling-wordpress-chat-transcript/)
* [文章类型、格式与分类法，我的天！](http://ottopress.com/2010/post-types-and-formats-and-taxonomies-oh-my/)
* [论标准化的文章格式](https://nacin.com/2011/01/27/on-standardized-post-formats/)
* [文章格式 vs. 文章类型](https://markjaquith.wordpress.com/2010/11/12/post-formats-vs-custom-post-types/)
* [更智能的文章格式？](https://dougal.gunters.org/blog/2010/12/10/smarter-post-formats/)
* [WordPress 主题支持生成器](https://generatewp.com/theme-support/)