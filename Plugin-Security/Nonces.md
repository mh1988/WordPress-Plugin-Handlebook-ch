Nonces are generated numbers used to verify origin and intent of requests for security purposes. Each nonce can only be used once

生成的数字用于验证请求的来源和意图，以用于安全目的。每个随机数只能使用一次

If your plugin allows users to submit data; be it on the Admin or the Public side; you have to make sure that the user is who they say they are and that they [have the necessary capability](https://developer.wordpress.org/plugins/security/checking-user-capabilities/) to perform the action. Doing both in tandem means that data is only changing when the user *expects* it to be changing.

如果你的插件允许用户提交数据;无论是管理员还是前台; 您必须确保用户是他本人，并且他们具有执行该操作的必要权限，两者并行意味着只有在用户期望数据发生变化时数据才会发生变化



## Using Nonces

Following our [checking user capabilities example](https://developer.wordpress.org/plugins/security/checking-user-capabilities/#restricted-to-a-specific-capability), the next step in user data submission security is using nonces.

检查完用户的权限之后，下一步就是在用户的提交的数据中使用随机数

The capability check ensures that only users who have permission to delete a post are able to delete a post. But what if someone were to trick you into clicking that link? You have the necessary capability, so you could unwittingly delete a post.

权限检查确保有权限删除文章的人才能删除。但是如果有人引诱你点击删除链接呢？你也有相应的权限，所以你就在不知不觉中删除了一篇文章。

Nonces can be used to check that the current user actually intends to perform the action.

可以使用Nonces来检查当前用户实际上是否打算执行该操作

When you generate the delete link, you’ll want to use [wp_create_nonce()](https://developer.wordpress.org/reference/functions/wp_create_nonce/) function to add a nonce to the link, the argument passed to the function ensures that the nonce being created is unique to that particular action.

在生成删除链接时，您需要使用wp_create_nonce（）函数向链接添加一个随机数，传递给该函数的参数确保创建的随机数对于该特定操作是唯一的

Then, when you’re processing a request to delete a link, you can check that the nonce is what you expect it to be

然后，当您处理删除链接的请求时，您可以检查该随机数是否与您期望的一致

For more information, Mark Jaquith’s [post on WordPress nonces](http://markjaquith.wordpress.com/2006/06/02/wordpress-203-nonces/) is a great resource.

有关更多信息，Mark Jaquith的帖子是一个很好的资源。

## Complete Example

Complete example using capability checks, data validation, secure input, secure output and nonces:

使用功能检查，数据验证，安全输入，安全输出和随机数的完整示例

```php
<?php
/**
 * generate a Delete link based on the homepage url
 */
function wporg_generate_delete_link($content)
{
    // run only for single post page
    if (is_single() && in_the_loop() && is_main_query()) {
        // add query arguments: action, post, nonce
        $url = add_query_arg(
            [
                'action' => 'wporg_frontend_delete',
                'post'   => get_the_ID(),
                'nonce'  => wp_create_nonce('wporg_frontend_delete'),
            ],
            home_url()
        );
        return $content . ' <a href="' . esc_url($url) . '">' . esc_html__('Delete Post', 'wporg') . '</a>';
    }
    return null;
}
 
/**
 * request handler
 */
function wporg_delete_post()
{
    if (
        isset($_GET['action']) &&
        isset($_GET['nonce']) &&
        $_GET['action'] === 'wporg_frontend_delete' &&
        wp_verify_nonce($_GET['nonce'], 'wporg_frontend_delete')
    ) {
 
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




