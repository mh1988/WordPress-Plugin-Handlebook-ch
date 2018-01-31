To add a new Top-level menu to WordPress Administration, use the [add_menu_page()](https://developer.wordpress.org/reference/functions/add_menu_page/) function.

要将新的顶级菜单添加到WordPress管理，请使用add_menu_page（）函数。

```
<?php
add_menu_page(
    string $page_title,
    string $menu_title,
    string $capability,
    string $menu_slug, 
    callable $function = '',
    string $icon_url = '',
    int $position = null
);
```

###  [Example](https://developer.wordpress.org/plugins/administration-menus/top-level-menus/#example)

Lets say we want to add a new Top-level menu called “WPOrg”.

现在我们想添加一个“WPOrg”的顶级菜单

**The first step** will be creating a function which will output the HTML. In this function we will perform the necessary security checks and render the options we’ve registered using the [Settings API](https://developer.wordpress.org/plugins/settings/).

**第一步**创建一个函数用来输出HTML。在这个函数中我们将执行安全检查和渲染我们用[Settings API](https://developer.wordpress.org/plugins/settings/).已经注册的options

>  Note:
>
> We recommend wrapping your HTML using a `<div>` with a class of `wrap`.
>
> 推荐用一个带有`wrap`class的`<div>`包裹这段 HTML

```php
<?php
function wporg_options_page_html()
{
    // check user capabilities
    if (!current_user_can('manage_options')) {
        return;
    }
    ?>
    <div class="wrap">
        <h1><?= esc_html(get_admin_page_title()); ?></h1>
        <form action="options.php" method="post">
            <?php
            // output security fields for the registered setting "wporg_options"
            settings_fields('wporg_options');
            // output setting sections and their fields
            // (sections are registered for "wporg", each field is registered to a specific section)
            do_settings_sections('wporg');
            // output save settings button
            submit_button('Save Settings');
            ?>
        </form>
    </div>
    <?php
}
```

**The second step** will be registering our WPOrg menu. The registration needs to occur during the `admin_menu` action hook.

**第二步** 去注册我们的 WPOrg 菜单。 通过 `admin_menu` 这个钩子完成

```
<?php
function wporg_options_page()
{
    add_menu_page(
        'WPOrg',
        'WPOrg Options',
        'manage_options',
        'wporg',
        'wporg_options_page_html',
        plugin_dir_url(__FILE__) . 'images/icon_wporg.png',
        20
    );
}
add_action('admin_menu', 'wporg_options_page');
```

 For a list of parameters and what each do please see the [add_menu_page()](https://developer.wordpress.org/reference/functions/add_menu_page/) in the reference.

关于这个[add_menu_page()](https://developer.wordpress.org/reference/functions/add_menu_page/) 里面的辣么多参数是干啥的，请点击进去浏览。

###  [用一个PHP文件输出代替html](https://developer.wordpress.org/plugins/administration-menus/top-level-menus/#using-a-php-file-for-html)

The best practice for portable code would be to create a Callback that requires/includes your PHP file.

想让你的代码具有可移植性？那你最好创建一个回调函数去引入这个PHP文件

For the sake of completeness and helping you understand legacy code, we will show another way: passing a `PHP file path` as the `$menu_slug` parameter with an `null` `$function` parameter.

为了完整性和帮助您理解代码，我们将使用另一种方式：将PHP文件路径作为$ menu_slug参数传递给一个函数。

```
<?php
function wporg_options_page()
{
    add_menu_page(
        'WPOrg',
        'WPOrg Options',
        'manage_options',
        plugin_dir_path(__FILE__) . 'admin/view.php',
        null,
        plugin_dir_url(__FILE__) . 'images/icon_wporg.png',
        20
    );
}
add_action('admin_menu', 'wporg_options_page');

```

## [删除顶级菜单](https://developer.wordpress.org/plugins/administration-menus/top-level-menus/#remove-a-top-level-menu)

To remove a registered menu from WordPress Administration, use the [remove_menu_page()](https://developer.wordpress.org/reference/functions/remove_menu_page/) function.

删除一个已经注册过的顶级菜单，用remove_menu_page()函数

```
<?php
remove_menu_page(
    string $menu_slug
);
```

Removing menus won’t prevent users accessing them directly.
This should never be used as a way to restrict [user capabilities](https://developer.wordpress.org/plugins/users/roles-and-capabilities/).

### Example [#Example](https://developer.wordpress.org/plugins/administration-menus/top-level-menus/#example)

Lets say we want to remove the “Tools” menu from.

```
<?php
function wporg_remove_options_page()
{
    remove_menu_page('tools.php');
}
add_action('admin_menu', 'wporg_remove_options_page', 99);

```

Make sure that the menu have been registered with the `admin_menu` hook before attempting to remove, specify a higher priority number for [add_action()](https://developer.wordpress.org/reference/functions/add_action/).