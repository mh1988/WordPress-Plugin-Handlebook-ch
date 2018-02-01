Now that we know how to create a [basic shortcode](https://developer.wordpress.org/plugins/shortcodes/basic-shortcodes/) and how to use it as [self-closing and enclosing](https://developer.wordpress.org/plugins/shortcodes/enclosing-shortcodes/), we will look at using parameters in shortcode `[$tag]` and handler function.

现在我们知道怎么创建[basic shortcode](https://developer.wordpress.org/plugins/shortcodes/basic-shortcodes/) 也知道怎么使用 自闭和和封闭标签，现在我们来看看怎么在段标签里面传参数

Shortcode `[$tag]` can accept parameters, known as attributes:

短标签里面可以接收参数，称为属性

```PHP
[wporg title="WordPress.org"]
Having fun with WordPress.org shortcodes.
[/wporg]
```

Shortcode handler function can accept 3 parameters:

相应的短标签处理函数可以接收三个参数

- `$atts` – array – [$tag] attributes
- `$content` – string – post content
- `$tag` – string – the name of the [$tag] (i.e. the name of the shortcode)

```
function wporg_shortcode($atts = [], $content = null, $tag = '') {}
```

##  [解析属性](https://developer.wordpress.org/plugins/shortcodes/shortcodes-with-parameters/#parsing-attributes)

For the user, shortcodes are just strings with square brackets inside the post content. The user have no idea which attributes are available and what happens behind the scenes.

对于用户来说，短标签只是在发布内容中带有方括号的字符串。用户不知道哪些属性可用以及幕后发生了什么

For plugin developers, there is no way to enforce a policy on the use of attributes. The user may include one attribute, two or none at all.

对于插件卡发着来说，也不知道在属性使用的时候，这个用户引入了几个属性

To gain control of how the shortcodes are used:

咋个办呢？

- Declare default parameters for the handler function         给处理函数定义默认参数
- Performing normalization of the key case for the attributes array with [array_change_key_case()](http://php.net/manual/en/function.array-change-key-case.php)              使用 [array_change_key_case()](http://php.net/manual/en/function.array-change-key-case.php) 对这个属性数组进行key的标准化，统一大小写
- Parse attributes using [shortcode_atts()](https://developer.wordpress.org/reference/functions/shortcode_atts/) providing default values array and user `$atts` 用[shortcode_atts()](https://developer.wordpress.org/reference/functions/shortcode_atts/) 解析属性，这个函数可以给出一个带有默认值的数组和用户的$atts进行合并
- [Secure the output](https://developer.wordpress.org/plugins/security/securing-output/) before returning it    在返回之前要对数据进行安全检查

## 一个完整的例子 [#](https://developer.wordpress.org/plugins/shortcodes/shortcodes-with-parameters/#complete-example)

Complete example using a basic shortcode structure, taking care of self-closing and enclosing scenarios, shortcodes within shortcodes and securing output.

完整的例子兼顾了自闭标签和闭合标签（不知道什么鬼？不知道怎么翻译），还用了输出

A [wporg] shortcode that will accept a title and will display a box that we can style with CSS.

这个短标签将接受一个 title 并且用div包裹起来，可以用css加样式

```PHP
<?php
function wporg_shortcode($atts = [], $content = null, $tag = '')
{
    // normalize attribute keys, lowercase 将数组的key转换为小写
    $atts = array_change_key_case((array)$atts, CASE_LOWER);
 
    // override default attributes with user attributes  重写默认值，用户的属性值会覆盖默认值
    $wporg_atts = shortcode_atts([
                                     'title' => 'WordPress.org',
                                 ], $atts, $tag);
 
    // start output
    $o = '';
 
    // start box
    $o .= '<div class="wporg-box">';
 
    // title
    $o .= '<h2>' . esc_html__($wporg_atts['title'], 'wporg') . '</h2>';
 
    // enclosing tags
    if (!is_null($content)) {
        // secure output by executing the_content filter hook on $content
        $o .= apply_filters('the_content', $content);
 
        // run shortcode parser recursively
        $o .= do_shortcode($content);
    }
 
    // end box
    $o .= '</div>';
 
    // return output
    return $o;
}
 
function wporg_shortcodes_init()
{
    add_shortcode('wporg', 'wporg_shortcode');
}
 
add_action('init', 'wporg_shortcodes_init');
```

