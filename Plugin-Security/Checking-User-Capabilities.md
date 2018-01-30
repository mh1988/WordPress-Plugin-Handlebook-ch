### 检测用户

如果你的插件允许用户提交数据，不管在后台还是公共菜单，都应该具备检查用户的功能

### 用户的角色和功能

创建有效的安全层最重要的一步就是要有一个用户权限系统。WordPress提供用户角色和功能的表单

每一个登录进入WordPress的用户会根据他们的用户角色自动给他们分配特殊的用户功能

`用户角色`只是一个名称，说明了用户属于哪个组。每个组都有特定的预定义功能集。

例如，你网站可以有一个管理者的角色，而其他用户可能有编辑或者作者的角色。你可以有多个管理者，即：你可以有2个管理者的角色。

**用户功能**是您分配给每个用户或者用户角色的特定权限

举个栗子，管理员有“管理设置”权限，可以浏览、编辑、保存设置；而编辑人员没有这种权限，以防他们更改设置。

这些权限之后会在管理范围内的不同功能接受验证。依据分配给角色的权限。菜单、功能以其他wp的体验(？)可以添加或删除。

在构建插件时，只有在当前用户具有必要的功能时，才能运行代码。

#### 用户体系

级别最高的用户角色，拥有更多的权限,在这个用户体系中每个用户角色继承上一任的权限.

例如`Administrator`,通常是网站的最高管理者，他有如下权限：“Subscriber”, “Contributor”, “Author” and “Editor”.

###  [无限制](https://developer.wordpress.org/plugins/security/checking-user-capabilities/#no-restrictions)

下面这个例子在前端创建了一个可以删除文件的连接。因为这个代码没有做权限检测，**所以任何访客都可以删除文件。**

```php
/**
 * generate a Delete link based on the homepage url
 */
function wporg_generate_delete_link($content)
{
    // run only for single post page
    if (is_single() && in_the_loop() && is_main_query()) {
        // add query arguments: action, post
        $url = add_query_arg(
            [
                'action' => 'wporg_frontend_delete',
                'post'   => get_the_ID(),
            ],
            home_url()
        );
        return $content . ' <a href="' . esc_url($url) . '">' . esc_html__('Delete Post', 'wporg') . '</a>;';
    }
    return null;
}
 
/**
 * request handler
 */
function wporg_delete_post()
{
    if (isset($_GET['action']) && $_GET['action'] === 'wporg_frontend_delete') {
 
        // verify we have a post id
        $post_id = (isset($_GET['post'])) ? ($_GET['post']) : (null);
 
        // verify there is a post with such a number
        $post = get_post((int)$post_id);
        if (empty($post)) {
            return;
        }
 
        // delete the post
        wp_trash_post($post_id);
 
        // redirect to admin page
        $redirect = admin_url('edit.php');
        wp_safe_redirect($redirect);
 
        // we are done
        die;
    }
}
 
if (current_user_can('edit_others_posts')) {
    /**
     * add the delete link to the end of the post content
     */
    add_filter('the_content', 'wporg_generate_delete_link');
 
    /**
     * register our request handler with the init hook
     */
    add_action('init', 'wporg_delete_post');
}
```

