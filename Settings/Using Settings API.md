## 添加配置 [#Adding Settings](https://developer.wordpress.org/plugins/settings/using-settings-api/#adding-settings)

You must define a new setting using [register_setting()](https://developer.wordpress.org/reference/functions/register_setting/), it will create an entry in the `{$wpdb->prefix}_options` table.

使用[register_setting](https://developer.wordpress.org/reference/functions/register_setting/) 创建配置，会为你创建一个 `{$wpdb->prefix}_options` 这种形式的表

You can add new sections on existing pages using [add_settings_section()](https://developer.wordpress.org/reference/functions/add_settings_section/).

你可以用这个添加一个sections

You can add new fields to existing sections using [add_settings_field()](https://developer.wordpress.org/reference/functions/add_settings_field/).

你可以用这个添加一个 fields

> [register_setting()](https://developer.wordpress.org/reference/functions/register_setting/) as well as the mentioned `add_settings_*()`functions should all be added to the `admin_init` action hook.
>
> 这些api 都应该添加到 `admin_init` 钩子中

### Add a Setting [#](https://developer.wordpress.org/plugins/settings/using-settings-api/#add-a-setting)

```
register_setting( 
    string $option_group, 
    string $option_name, 
    callable $sanitize_callback = ''
);
```

Please refer to the Function Reference about [register_setting()](https://developer.wordpress.org/reference/functions/register_setting/) for full explanation about the used parameters.

有关使用参数的完整说明，请参阅关于register_setting（）的函数参考。

### Add a Section [#](https://developer.wordpress.org/plugins/settings/using-settings-api/#add-a-section)

```
add_settings_section( 
    string $id, 
    string $title, 
    callable $callback, 
    string $page
);
```

Sections are the groups of settings you see on WordPress settings pages with a shared heading. In your plugin you can add new sections to existing settings pages rather than creating a whole new page. This makes your plugin simpler to maintain and creates fewer new pages for users to learn.

Sections是您在WordPress设置页面上看到的具有共享标题的设置组.在你的插件中，你可以添加新的sections到现有的设置页面，而不是创建一个全新的页面。这使得您的插件更容易维护，不要创建那么多页面，用户还得学习怎么使用

Please refer to the Function Reference about [add_settings_section()](https://developer.wordpress.org/reference/functions/add_settings_section/) for full explanation about the used parameters.

有关使用参数的完整说明，请参阅关于add_settings_section（）的函数参考。

```
add_settings_field(
    string $id, 
    string $title, 
    callable $callback, 
    string $page, 
    string $section = 'default', 
    array $args = []
);
```

Please refer to the Function Reference about [add_settings_field()](https://developer.wordpress.org/reference/functions/add_settings_field/) for full explanation about the used parameters.

有关使用参数的完整说明，请参阅关于add_settings_field（）的函数参考

### Example [#](https://developer.wordpress.org/plugins/settings/using-settings-api/#example)

```
<?php
function wporg_settings_init()
{
    // register a new setting for "reading" page
    register_setting('reading', 'wporg_setting_name');
 
    // register a new section in the "reading" page
    add_settings_section(
        'wporg_settings_section',
        'WPOrg Settings Section',
        'wporg_settings_section_cb',
        'reading'
    );
 
    // register a new field in the "wporg_settings_section" section, inside the "reading" page
    add_settings_field(
        'wporg_settings_field',
        'WPOrg Setting',
        'wporg_settings_field_cb',
        'reading',
        'wporg_settings_section'
    );
}
 
/**
 * register wporg_settings_init to the admin_init action hook
 */
add_action('admin_init', 'wporg_settings_init');
 
/**
 * callback functions
 */
 
// section content cb
function wporg_settings_section_cb()
{
    echo '<p>WPOrg Section Introduction.</p>';
}
 
// field content cb
function wporg_settings_field_cb()
{
    // get the value of the setting we've registered with register_setting()
    $setting = get_option('wporg_setting_name');
    // output the field
    ?>
    <input type="text" name="wporg_setting_name" value="<?php echo isset( $setting ) ? esc_attr( $setting ) : ''; ?>">
    <?php
}
```

## Getting Settings [#](https://developer.wordpress.org/plugins/settings/using-settings-api/#getting-settings)

```
get_option(
    string $option,
    mixed $default = false
);
```

Getting settings is accomplished with the [get_option()](https://developer.wordpress.org/reference/functions/get_option/) function.
The function accepts two parameters: the name of the option and an optional default value for that option.

```
// get the value of the setting we've registered with register_setting()
$setting = get_option('wporg_setting_name');
```

