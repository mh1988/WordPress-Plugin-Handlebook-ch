Securing input is the process of sanitizing (cleaning, filtering) input data.

安全输入是清洗过滤输入的数据

You use sanitizing when you don’t know what to expect or you don’t want to be strict with [data validation](https://developer.wordpress.org/plugins/security/data-validation/).

当你不知道你期望的数据或者不想用严格模式过滤数据，你可以使用 sanitizing

**Any time you’re accepting potentially unsafe data, it is important to validate or sanitize it.**

任何时候你要接收不安全的数据，非常重要的一定就是验证和清洗

##  [清洗数据](https://developer.wordpress.org/plugins/security/securing-input/#sanitizing-the-data)

The easiest way to sanitize data is with built-in WordPress functions.

WordPress内置了一些简单的清洗数据的函数

The `sanitize_*()` series of helper functions are super nice, as they ensure you’re ending up with safe data, and they require minimal effort on your part:

sanitize _ *（）系列的helper函数非常棒，因为它们能确保您的数据安全：

- [sanitize_email()](https://developer.wordpress.org/reference/functions/sanitize_email/)
- [sanitize_file_name()](https://developer.wordpress.org/reference/functions/sanitize_file_name/)
- [sanitize_html_class()](https://developer.wordpress.org/reference/functions/sanitize_html_class/)
- [sanitize_key()](https://developer.wordpress.org/reference/functions/sanitize_key/)
- [sanitize_meta()](https://developer.wordpress.org/reference/functions/sanitize_meta/)
- [sanitize_mime_type()](https://developer.wordpress.org/reference/functions/sanitize_mime_type/)
- [sanitize_option()](https://developer.wordpress.org/reference/functions/sanitize_option/)
- [sanitize_sql_orderby()](https://developer.wordpress.org/reference/functions/sanitize_sql_orderby/)
- [sanitize_text_field()](https://developer.wordpress.org/reference/functions/sanitize_text_field/)
- [sanitize_title()](https://developer.wordpress.org/reference/functions/sanitize_title/)
- [sanitize_title_for_query()](https://developer.wordpress.org/reference/functions/sanitize_title_for_query/)
- [sanitize_title_with_dashes()](https://developer.wordpress.org/reference/functions/sanitize_title_with_dashes/)
- [sanitize_user()](https://developer.wordpress.org/reference/functions/sanitize_user/)
- [esc_url_raw()](https://developer.wordpress.org/reference/functions/esc_url_raw/)
- [wp_filter_post_kses()](https://developer.wordpress.org/reference/functions/wp_filter_post_kses/)
- [wp_filter_nohtml_kses()](https://developer.wordpress.org/reference/functions/wp_filter_nohtml_kses/)



##  [Example](https://developer.wordpress.org/plugins/security/securing-input/#example)

一个title字段

```php
<input id="title" type="text" name="title">
```

You can sanitize the input data with the [sanitize_text_field()](https://developer.wordpress.org/reference/functions/sanitize_text_field/) function:

你可以用 sanitize_text_field() 函数清洗数据

```php
$title = sanitize_text_field($_POST['title']);
update_post_meta($post->ID, 'title', $title);
```

Behind the scenes, [sanitize_text_field()](https://developer.wordpress.org/reference/functions/sanitize_text_field/) does the following:

这个函数做了如下的操作

- Checks for invalid UTF-8 检查是否有效的UTF-8字符
- Converts single less-than characters (<) to entity 将单个小于号字符（<）转换为实体
- Strips all tags 丢掉标签
-  Removes line breaks, tabs and extra white space 移除换行符，tabs 和 额外的空格
- Strips octets