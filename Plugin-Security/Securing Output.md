Securing output is the process of escaping output data.

安全输入就是转义输出数据

Escaping means stripping out unwanted data, like malformed HTML or script tags.

转义的意思就是丢掉不需要的数据，类似html标签或者script脚本标记

**Whenever you’re rendering data, make sure to properly escape it. Escaping output prevents XSS (Cross-site scripting) attacks.**

无论何时渲染数据，请确保正确地将其转义。转义输出可防止XSS（跨站点脚本）攻击。

> [Cross-site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting) is a type of computer security vulnerability typically found in web applications. XSS enables attackers to inject client-side scripts into web pages viewed by other users. A cross-site scripting vulnerability may be used by attackers to bypass access controls such as the same-origin policy.
>
> Cross-site scripting (XSS) 是一种通常在Web应用程序中发现的计算机安全漏洞,XSS使攻击者可以将客户端脚本注入到其他用户查看的网页中,攻击者可能会使用跨站脚本漏洞绕过访问控制，如同源策略

#####  [转义（Escaping）](https://developer.wordpress.org/plugins/security/securing-output/#escaping)

Escaping helps securing your data prior to rendering it for the end user.

转义有助于在为最终用户呈现数据之前保护您的数据

 WordPress has a few helper functions you can use for most common scenarios.

WordPress有一些帮助函数，你可以用在很多常见的场景中（下面的函数就不翻译了）

- [esc_html()](https://developer.wordpress.org/reference/functions/esc_html/) – Use this function anytime an HTML element encloses a section of data being displayed.
- [esc_url()](https://developer.wordpress.org/reference/functions/esc_url/) – Use this function on all URLs, including those in the `src` and `href` attributes of an HTML element.
- `esc_js()`– Use this function for inline Javascript.
- [esc_attr()](https://developer.wordpress.org/reference/functions/esc_attr/) – Use this function on everything else that’s printed into an HTML element’s attribute.

> Most WordPress functions properly prepare data for output, so you don’t need to escape the data again. For example, you can safely call [the_title()](https://developer.wordpress.org/reference/functions/the_title/) without escaping.
>
> 很多WordPress函数已经具备了转义功能，所以你不需要重复转义 一遍了，例如`the_title()`

#####本地化转义

Rather than using `echo` to output data, it’s common to use the WordPress localization functions, such as [_e()](https://developer.wordpress.org/reference/functions/_e/) or [__()](https://developer.wordpress.org/reference/functions/__/).

不要使用 echo 输出数据，可以用本地化函数 例如[_e()](https://developer.wordpress.org/reference/functions/_e/) or [__()](https://developer.wordpress.org/reference/functions/__/).

These functions simply wrap a localization function inside an escaping function

这些函数只是将本地化函数封装在转义函数里面

```
esc_html_e( 'Hello World', 'text_domain' );
// same as
echo esc_html( __( 'Hello World', 'text_domain' ) );
```

These helper functions combine localization and escaping:

这些帮助函数结合了本地化和转义

- [esc_html__()](https://developer.wordpress.org/reference/functions/esc_html__/)
- [esc_html_e()](https://developer.wordpress.org/reference/functions/esc_html_e/)
- [esc_html_x()](https://developer.wordpress.org/reference/functions/esc_html_x/)
- [esc_attr__()](https://developer.wordpress.org/reference/functions/esc_attr__/)
- [esc_attr_e()](https://developer.wordpress.org/reference/functions/esc_attr_e/)
- [esc_attr_x()](https://developer.wordpress.org/reference/functions/esc_attr_x/)

##### Custom Escaping

In the case that you need to escape your output in a specific way, the function [wp_kses()](https://developer.wordpress.org/reference/functions/wp_kses/) (pronounced “kisses”) will come in handy.

如果您需要以特定方式转义您的输出，函数wp_kses（）（发音为“kisses”）将派上用场。

This function makes sure that only the specified HTML elements, attributes, and attribute values will occur in your output, and normalizes HTML entities.

此函数确保只有指定的HTML元素，属性和属性值会出现在您的输出中，并规范化HTML实体

```
$allowed_html = [
        'a'     => []
        'href'  => [],
        'title' => [],
    ],
    'br'     => [],
    'em'     => [],
    'strong' => [],
];
echo wp_kses( $custom_content, $allowed_html );
```

[wp_kses_post()](https://developer.wordpress.org/reference/functions/wp_kses_post/) is a wrapper function for wp_kses where `$allowed_html` is a set of rules used by post content.

wp_kses_post（）是wp_kses的包装函数，其中$ allowed_html是由发布内容使用的一组规则。

```
echo wp_kses_post( $post_content );
```

