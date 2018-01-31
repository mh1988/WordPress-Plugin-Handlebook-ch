## Add a Shortcode [#Add a Shortcode](https://developer.wordpress.org/plugins/shortcodes/basic-shortcodes/#add-a-shortcode)

It is possible to add your own shortcodes by using the Shortcode API. The process involves registering a callback `$func` to a shortcode `$tag`using [add_shortcode()](https://developer.wordpress.org/reference/functions/add_shortcode/).

可以用 `Shortcode` API 创建一个你自己额 `Shortcode` , 这个通过使用 `add_shortcode()`这个函数注册一个回调函数到 `$tag`这个参数上

```
<?php
add_shortcode(
    string $tag,
    callable $func
);
```

### 在主题里面 [#](https://developer.wordpress.org/plugins/shortcodes/basic-shortcodes/#in-a-theme)

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

`[wporg]` is your new shortcode. The use of the shortcode will trigger the `wporg_shortcode` callback function.

`[wporg]` 就是你创建的 shortcode，可以用这个 `[wporg]`  触发 `wporg_shortcode` 这个函数

###  [在插件中](https://developer.wordpress.org/plugins/shortcodes/basic-shortcodes/#in-a-plugin)

Unlike a Theme, a Plugin is run at a very early stage of the loading process thus requiring us to postpone the adding of our shortcode until WordPress has been initialized.

跟在主题中使用的方式不同，插件往往执行的比较早因此需要在wordpress加载完毕之后再添加shortcode

We recommend the `init` action hook.

推荐使用`init`这个钩子

```
<?php
function wporg_shortcodes_init()
{
    function wporg_shortcode($atts = [], $content = null)
    {
        // do something to $content
 
        // always return
        return $content;
    }
    add_shortcode('wporg', 'wporg_shortcode');
}
add_action('init', 'wporg_shortcodes_init');

```

## Remove a Shortcode [#Remove a Shortcode](https://developer.wordpress.org/plugins/shortcodes/basic-shortcodes/#remove-a-shortcode)

It is possible to remove shortcodes by using the Shortcode API. The process involves removing a registered `$tag` using [remove_shortcode()](https://developer.wordpress.org/reference/functions/remove_shortcode/).

可以用 Shortcode API 移除 shortcodes。 用`remove_shortcode()` 就可以移除掉已经注册在 `$tag` 上面的回调函数

```
<?php
remove_shortcode(
    string $tag
);
```

Make sure that the shortcode have been registered before attempting to remove. Specify a higher priority number for [add_action()](https://developer.wordpress.org/reference/functions/add_action/) or hook into an action hook that is run later.

在尝试移除之前请确认这个 shortcode 已经注册。为add_action（）指定更高的优先级编号，或者挂到一个稍后要执行的钩子上。

##  [检查shortcode是否存在](https://developer.wordpress.org/plugins/shortcodes/basic-shortcodes/#check-if-a-shortcode-exists)

To check whether a shortcode has been registered use [shortcode_exists()](https://developer.wordpress.org/reference/functions/shortcode_exists/).

用`shortcode_exists()` 检测一个 shortcode是否已经注册。