The Settings API, added in WordPress 2.7, allows admin pages containing settings forms to be managed semi-automatically. It lets you define settings pages, sections within those pages and fields within the sections.

在WordPress 2.7中增加的设置API允许半自动地管理包含设置表单的管理页面。它可以让你定义设置页面，这些页面内的部分和部分内的字段

New settings pages can be registered along with sections and fields inside them. Existing settings pages can also be added to by registering new settings sections or fields inside of them.

新的设置页面随着 sections 和 fields 的注入被创建出来。已经存在的设置页面也可以注册新的 设置 sections 和 fields 

Organizing registration and validation of fields still requires some effort from developers, but avoids a lot of complex debugging of underlying options management.

组织领域的注册和验证仍然需要开发者的一些努力，但是避免了底层选项管理的大量复杂调试

> When using the Settings API, the form POST to `wp-admin/options.php` which provides fairly strict capabilities checking. Users will need the `manage_options` capability (and in Multisite will have to be a Super Admin) to submit the form.
>
> 当使用Settings API时，表单POST到wp-admin / options.php，它提供了相当严格的功能检查。用户将需要manage_options功能（在Multisite中必须是超级管理员）才能提交表单。

## 为什么要使用 Setting API? [#Why Use the Setting API?](https://developer.wordpress.org/plugins/settings/settings-api/#why-use-the-setting-api)

A developer *could* ignore this API and write their own settings page without it. That begs the question, what benefit does this API bring to the table? Following is a quick rundown of some of the benefits.

开发人员可以忽略这个API，并在没有它的情况下编写自己的设置页。这引出了一个问题，这个API带来了什么好处？以下是一些好处的简要说明。

### 视觉一致化 [#Visual Consistency](https://developer.wordpress.org/plugins/settings/settings-api/#visual-consistency)

Using the API to generate your interface elements guarantees that your settings page will look like the rest of the administrative content. Have you ever seen a plugin settings page that looked like it was designed by a 5-year-old? You can bet that developer didn’t use the API. So, one strong argument is that your interface will look like it belongs, and thanks to the talented team of WordPress designers, it’ll look awesome!

用api生成的界面元素看起来跟wordpress的管理后台是一样的。你有没有看到一个插件的设置界面看起来就像一个5岁的娃娃设计的一样？如果有你可以肯定他没有使用 API，所以这是一个有力的证据表明你应该使用API创建界面，并且应该感谢wordpress天才的设计师团队。他们非常牛逼💯

### 稳定性 (Future-Proofing!) [#Robustness (Future-Proofing!)](https://developer.wordpress.org/plugins/settings/settings-api/#robustness-future-proofing)

Since the API is part of WordPress Core, any updates will automatically consider your plugin’s settings page. If you go off the reservation and make your own interface, WordPress Core updates are more likely to break your customizations. There is also a wider audience testing and maintaining that API code, so it will tend to be more stable.

由于API是WordPress Core的一部分，任何更新都会自动考虑您的插件的设置页面，如果您非要自己创建自己的界面，则WordPress Core更新可能会破坏您的自定义设置，而测试和维护这些API代码的用户也越来越多，所以它会更加稳定。

### 花少量的工作! [#Less Work!](https://developer.wordpress.org/plugins/settings/settings-api/#less-work)

Of course the most immediate benefit is that the WordPress API does a lot of work for you under the hood. Here are a few examples of things the Settings API does besides applying an awesome-looking, integrated design.

当然，最直接的好处是WordPress的API在你面前为你做了很多工作。以下是Settings API除了应用一个令人敬畏的集成设计之外的几个例子

- **Handling Form Submissions –** Let WordPress handle retrieving and storing your $_POST submissions. 处理表单提交 - 让WordPress处理检索并存储您的$ _POST提交
- **Include Security Measures –** You get extra security measures such as nonces, etc. for free. 包括安全措施 - 您可以免费获得额外的安全措施，如随机等
- **Sanitizing Data –** You get access to the same methods that the rest of WordPress uses for ensuring strings are safe to use. 您可以访问与WordPress剩余部分使用的相同方法，以确保字符串可以安全使用

## Function Reference

##### Setting Register/Unregister

[register_setting()](https://developer.wordpress.org/reference/functions/register_setting/)

[unregister_setting()](https://developer.wordpress.org/reference/functions/unregister_setting/)

##### Add Field/Section

[add_settings_section()](https://developer.wordpress.org/reference/functions/add_settings_section/)

[add_settings_field()](https://developer.wordpress.org/reference/functions/add_settings_field/)

##### Options Form Rendering

[settings_fields()](https://developer.wordpress.org/reference/functions/settings_fields/)

[do_settings_sections()](https://developer.wordpress.org/reference/functions/do_settings_sections/)

[do_settings_fields()](https://developer.wordpress.org/reference/functions/do_settings_fields/)

#####Errors

[add_settings_error()](https://developer.wordpress.org/reference/functions/add_settings_error/)

[get_settings_errors()](https://developer.wordpress.org/reference/functions/get_settings_errors/)

[settings_errors()](https://developer.wordpress.org/reference/functions/settings_errors/)


