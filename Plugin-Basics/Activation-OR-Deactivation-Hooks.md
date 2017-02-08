### 激活 / 取消激活 钩子

  当你需要开启和关闭插件的时候，Activation【激活钩子】 和 deactivation【禁用钩子】 hooks 提供给你执行这个动作的的方式.

  (activation)使用【激活钩子】的时候，插件可以自己写逻辑代码添加重写规则，添加自定义数据表或者设置一些默认选项的值。

  (deactivation)使用【禁用钩子】的时候，插件可以写逻辑代码移除临时数据，例如缓存和临时文件目录等。

> 注意：
>
> 这个(deactivation)【禁用钩子】有时候跟卸载的钩子分不清楚。卸载钩子更适合用于永久删除所有数据，例如删除插件选项和自定义数据表，等等。

Activation【激活钩子】

使用激活钩子，用`register_activation_hook()` function:

```php
register_activation_hook(__FILE__,'pluginperfix_function_to_run')
```

Deactivation【禁用钩子】

使用禁用钩子，用`register_deactivation_hook()` function:

```php
register_deactivation_hook( __FILE__, 'pluginprefix_function_to_run' );
```

每个函数的第一个参数是你的主插件文件，就是那个放了插件头部注释的文件。通常这两个函数将在你的主插件文件中被触发；但是，如果函数放在其他任何文件中，则必须更新第一个参数才能指向正确的主插件文件

例子：

一个激活钩子最常见的用途之一就是更新WordPress插件永久链接时注册一个自定义文章类型。这就摆脱了讨厌的404错误。

一起看下这个例子：

```php
function pluginprefix_setup_post_types()
{
  //register the "book" custom post type
  register_post_type('book', ['public' => 'true']);
}
add_action('init', 'pluginprefix_setup_post_type');

function pluginprefix_install()
{
  
}  
```

深入了解关于`activation`和`deactivation` 钩子信息，这里有一些好的资源：

* [`register_activation_hook()`](https://developer.wordpress.org/reference/functions/register_activation_hook/)
* [`register_deactivation_hook()`](https://developer.wordpress.org/reference/functions/register_deactivation_hook/)

