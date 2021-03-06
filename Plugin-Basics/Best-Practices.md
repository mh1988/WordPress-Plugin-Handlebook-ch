## 高级练习

这儿有一些高级练习帮助组织你的代码，这样就可以更好的在WrodPress核心和其他插件身边运行。

#### 避免命名冲突

当你的变量、方法用了相同的名称或者类用了其他插件的相同的名称，就会发生命名冲突

幸运的是，你可以用下面的方法避免命名冲突

#### 有关程序的

默认情况下，所有变量，函数和类被其他插件在全局环境下已经定义了，这就意味着其他插件内部可能会重写你插件中的变量、函数和类反之亦然。变量如果定义在函数体内或者类内部不会受到这种影响。

#### 前缀

所有的变量，函数和类应该使用一个唯一的前缀标识。前缀可以防止其他插件重写你的变量或者意外调用你的函数和类。它也会阻止你的插件做同样的事。

##### 检查是否存在

PHP提供一些函数用于验证变量，函数，类和常量是否存在，它们通常返回一个标记。如果函数检验确实存在就会返回`true`

* Variables: `isset()`(includes arrays, objects, etc.)
* Functions:`function_exists()`
* Classes: `class_exists()`
* Constants: `defined()`;

#### 例子

```php
<?php
  //create a function called "wporg_init" if it doesn't alreay exist
  if(!function_exists('wporg_init')) {
    function wporg_init() {
      register_setting('wporg_settings', 'wporg_option_foo');
    }
  }

 if(!function_exists('wporg_get_foo'))	{
   function wporg_get_foo() {
     return get_option('wporg_option_foo');
   }
 }
```

#### OOP

一种简单的方法处理命名冲突的问题就是在你的插件代码中使用类

你仍然需要检查这个类是否存在，在PHP中类中的会被保护起来

例子：

```php
<?php 
if(!class_exists('WPOrg_Plugin')) {
  class WPOrg_Plugin 
  {
    public static function init() {
      register_setting('wporg_settings', 'wporg_option_foo');
    }
    
    publif static function get_foo() {
      return get_option('wporg_option_foo');
    }
  }
  
  WPOrg_Plugin::init();
  WPOrg_Plugin::get_foo();
}
```



#### 文件组织

插件的根目录里面只包含你的 plugin-name.php文件和optionally, 你的uninstall.php文件，其他所有文件在任何可能的情况下都组织到子文件夹内。

#### 文件夹结构

一个明确的文件夹结构可以帮助你更好的工作

这里有一个简单的文件夹结构供你参考引用：

```
/plugin-name
	plugin-name.php
	uninstall.php
	/languages
	/includes
	/admin
		/js
		/css
		/images
	/public
		/js
		/css
		/images
```

#### 插件结构

结构或者代码组织，取决于你的插件大小

对于小的插件，WordPress的核心作用有限，主题或者其他插件，工程复杂的类没有效益，除非你知道插件以后会扩展的很大

对于代码量比较大的插件，开始就用类的思路。样式和脚本文件分别建立文件夹存放。将有助于代码的组织和插件的长期维护

#### 选择性加载

帮助你从公用代码中分离你的admin代码。用这个条件`is_admin()`

例子：

```php
<?php 
	if(is_admin()) {
      require_once(dirname(__FILE__ . '/admin/plugin-name-admin.php'));
	}
```

### 结构模式 [#结构模式](https://developer.wordpress.org/plugins/the-basics/best-practices/#architecture-patterns)

这有很多合理的结构模式，他们大致可以分为三类：

- [单个文件，包含一些 functions](https://github.com/GaryJones/move-floating-social-bar-in-genesis/blob/master/move-floating-social-bar-in-genesis.php)
- [单个文件，包含一个类，实例的对象和一些functions](https://github.com/norcross/wp-comment-notes/blob/master/wp-comment-notes.php)
- [主要的插件文件，和一个或者更多的类文件](https://github.com/tommcfarlin/WordPress-Plugin-Boilerplate)

### Architecture Patterns Explained [#Architecture Patterns Explained](https://developer.wordpress.org/plugins/the-basics/best-practices/#architecture-patterns-explained)

以上代码组织中更复杂的具体实现已经被编写为教程和幻灯片:

- [Slash – Singletons, Loaders, Actions, Screens, Handlers](http://jaco.by/2012/12/12/slash-architecture-my-approach-to-building-wordpress-plugins/)
- [An MVC Inspired Approach to WordPress Plugin Development](http://www.renegadetechconsulting.com/tutorials/an-mvc-inspired-approach-to-wordpress-plugin-development)
- [Implementing the MVC Pattern in WordPress Plugins](http://iandunn.name/wp-mvc)

[](https://developer.wordpress.org/plugins/the-basics/best-practices/#top)

## 插件模板（可理解为框架） [#Boilerplate Starting Points](https://developer.wordpress.org/plugins/the-basics/best-practices/#boilerplate-starting-points)

是不是每次创建新插件都要从头开始整理思绪？或许你可以使用框架。框架的优势在于可以保持多个插件之间结构一致性。 框架也可以让其他人更容易为你的插件贡献代码，因为你用的这个框架他们也很熟悉。

一些不太一样，但看上去还不错的值得深入的了解的案例

- [WordPress Plugin Boilerplate](https://github.com/tommcfarlin/WordPress-Plugin-Boilerplate): 一个wordpress插件开发基础框架，提供一个干净并且一致性的指南方便你开发你的插件
- [WordPress Plugin Bootstrap](https://github.com/claudiosmweb/wordpress-plugin-boilerplate): Basic bootstrap to develop WordPress plugins using Grunt, Compass, GIT, and SVN.
- [WP Skeleton Plugin](https://github.com/ptahdunbar/wp-skeleton-plugin): Skeleton plugin that focuses on unit tests and use of composer for development.
- [General search for WordPress plugin boilerplates on GitHub](https://github.com/search?q=wordpress+plugin+boilerplate&ref=reposearch)

Of course, you could take different aspects of these and others to create your own custom boilerplate.