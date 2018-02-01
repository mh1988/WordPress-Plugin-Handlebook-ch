# Enclosing Shortcodes

The are two scenarios for using shortcodes:

shortcodes 有两种使用场景

- The shortcode is a self-closing tag like we seen in the [Basic Shortcodes](https://developer.wordpress.org/plugins/shortcodes/basic-shortcodes/) section.
- 一种是在[Basic Shortcodes](https://developer.wordpress.org/plugins/shortcodes/basic-shortcodes/) 中介绍的
- The shortcode is enclosing content.
- 一种是包含一段内容

## Enclosing Content [#Enclosing Content](https://developer.wordpress.org/plugins/shortcodes/enclosing-shortcodes/#enclosing-content)

Enclosing content with a shortcode allows manipulations on the enclosed content.

示例如下

```
[wporg]content to manipulate[/wporg]
```

As seen above, all you need to do in order to enclose a section of content is add a beginning `[$tag]` and an end `[/$tag]`, similar to HTML

上面这段代码看到了吧，你可以用两个段标签包裹一段内容，有点类似HTML

## Processing Enclosed Content 处理包裹的内容	

Lets get back to our original [wporg] shortcode code:

我们再看看之前我们的那段代码

```
<?php
function wporg_shortcode($atts = [], $content = null)
{
    // do something to $content
 
    // always return
    return $content;
}
add_shortcode('wporg', 'wporg_shortcode');

```

Looking at the callback function we see that we chose to accept two parameters, `$atts` and `$content`. The `$content` parameter is going to hold our enclosed content. We will talk about `$atts` later.

来看看这个回调函数，我们允许接收2个参数，分别是`$atts` and `$content`，这个`$content`就是我们之前用段标签包裹的那段内容。稍后我们再讨论一下`$atts`

The default value of `$content` is set to `null` so we can differentiate between a self-closing tag and enclosing tags by using PHP function [is_null()](http://php.net/is_null).

 `$content`的默认值设为`null`，所以我们可以通过使用PHP函数is_null（）来区分自闭标签和封闭标签。

The shortcode `[$tag]`, including its content and the end `[/$tag]` will be replaced with the **return value** of the handler function.

短标签将在函数的返回值中被替换掉

> It is the responsibility of the handler function to [secure the output](https://developer.wordpress.org/plugins/security/securing-output/).
>
> 处理函数的责任是保证输出的安全。

## Shortcode-ception [#](https://developer.wordpress.org/plugins/shortcodes/enclosing-shortcodes/#shortcode-ception)

The shortcode parser performs a single pass on the content of the post.

短标签解析器在文章内容执行一次传递（大概意思就是说段标签包含的内容只能作为一次传递给回调函数，无法执行嵌套处理）

This means that if the `$content` parameter of a shortcode handler contains another shortcode, it won’t be parsed.

这意味着如果这段内容中包含其他段标签，是不会被解析的

```
[wporg]another [shortcode] is included[/wporg]
```

Using shortcodes inside other shortcodes is possible by calling [do_shortcode()](https://developer.wordpress.org/reference/functions/do_shortcode/) on the **final return value** of the handler function.

想处理也不是不可能，可以在最终返回值之前使用[do_shortcode()](https://developer.wordpress.org/reference/functions/do_shortcode/)函数

```
<?php
function wporg_shortcode($atts = [], $content = null)
{
    // do something to $content
 
    // run shortcode parser recursively
    $content = do_shortcode($content);
 
    // always return
    return $content;
}
add_shortcode('wporg', 'wporg_shortcode');

```

## Limitations

The shortcode parser is unable to handle mixing of enclosing and non-enclosing forms of the same `[$tag]`.

短标签解析器无法同时处理两种形式的。

```
[wporg] non-enclosed content [wporg]enclosed content[/wporg]
```

Instead of being treated as two shortcodes separated by the text “` non-enclosed content `“, the parser treats this as a single shortcode enclosing “`non-enclosed content [wporg]enclosed content`“.