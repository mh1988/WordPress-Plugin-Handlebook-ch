正如[开始](https://developer.wordpress.org/plugin/the-basics/#getting-started)描述的那样，头部描述是用来告诉Wordpress这个文件是插件。

在一个头部描述中至少要包含这个插件的名字，但是通常都会包含以下的一些内容：

- **Plugin Name:** (*required*)  插件名称，它将在后台的wordpress插件列表中显示
- **Plugin URI:** 插件主页，最好是你的网站地址，因为他必须是唯一的，不可以使用Wordpress.org地址
- **Description:**简短描述插件，将展示在wordpress后台。不能超过140个字符
- **Version:** 插件的版本号，例如1.0或者1.0.3

>  在为项目分配一个版本号时，请记住Wordpress使用PHP的`version_compare()`函数比较插件版本号。因此，在你发布插件的新版本时候，新版本号要大于旧版本号，例如，1.02事实上大于1.1

- **Author:** 插件作者名称，多个作者可以用逗号隔开
- **Author URI:**作者网站地址或者其他网站地址，例如Wordpress.org.
- **License:** 插件的授权(例如：GPL2)。更多权限信息可以看这里  [WordPress.org guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/). 
- **License URI:** 授权的许可的链接 (e.g. <https://www.gnu.org/licenses/gpl-2.0.html>).
- **Text Domain:** The [gettext](https://www.gnu.org/software/gettext/) text domain of the plugin. More information can be found in the [Text Domain](https://developer.wordpress.org/plugins/internationalization/how-to-internationalize-your-plugin/#text-domains) section of the [如何国际化插件](https://developer.wordpress.org/plugins/internationalization/how-to-internationalize-your-plugin/) page.
- **Domain Path:** 这个地址让wordpress知道译文在哪儿. 更多信息请看这里 in the [Domain Path](https://developer.wordpress.org/plugins/internationalization/how-to-internationalize-your-plugin/#domain-path) section of the [如何国际化插件](https://developer.wordpress.org/plugins/internationalization/how-to-internationalize-your-plugin/) page.

 一个有效的头部文件像下面这样👇

```
<?php
/*
Plugin Name:  WordPress.org Plugin
Plugin URI:   https://developer.wordpress.org/plugins/the-basics/
Description:  Basic WordPress Plugin Header Comment
Version:      20160911
Author:       WordPress.org
Author URI:   https://developer.wordpress.org/
License:      GPL2
License URI:  https://www.gnu.org/licenses/gpl-2.0.html
Text Domain:  wporg
Domain Path:  /languages
*/
```

