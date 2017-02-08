### 卸载/删除 方法



当你的插件需要从网站卸载之后或许需要做一些清理工作。

如果用户禁用这个插件是考虑卸载掉该插件，可以在WordPress后台的找到该插件点击删除。

当你的插件卸载之后，你需要清理所有插件选项和插件的特殊设置，以及一些数据库配置（可理解为清除残留配置数据等）

经验不足的开发者有时会在用`deactivation` hook 的作用上犯错误

这个表格对`deactivation`和`uninstall`两个钩子做了对比说明

| -                            | Deactivation Hook | Uninstall Hook |
| ---------------------------- | ----------------- | -------------- |
| 刷新缓存/临时                      | YES               | NO             |
| 刷新永久链接                       | YES               | NO             |
| 从{$wpdb-prefix}_options 移除配置 | NO                | YES            |
| 移除数据表                        | NO                | YES            |

#### 方法1：register_uninstall_hook

创建一个卸载钩子，用`register_uninstall_hook()`函数：

```php
register_uninstall_hook(__FILE__, 'pluginprefix_function_to_run')
```

#### 方法2：uninstall.php

使用本方法需要在你的插件根目录创建一个`uninsall.php`的文件. 当用户删除插件的时候这个魔术文件会自动运行 .

例子：/plugin-name/uninstall.php

>当用 uninstall.php，在执行之前，插件总是会检查常量`WP_UNINSALL_PLUGIN`以防止直接访问意外删除插件
>
>这个常量定义了WordPress在什么时间调用uninstall.php
>
>当执行`register_uninstall_hook()`卸载时,这个常量是没有定义的

下面的例子演示了删除条目和删除数据表：

```php
// if uninstall.php is not called by WordPress, die
if (!defined('WP_UNINSTALL_PLUGIN')) {
    die;
}
 
$option_name = 'wporg_option';
 
delete_option($option_name);
 
// for site options in Multisite
delete_site_option($option_name);
 
// drop a custom database table
global $wpdb;
$wpdb->query("DROP TABLE IF EXISTS {$wpdb->prefix}mytable");

```

> 注意：遍历所有博客删除选项非常占用资源