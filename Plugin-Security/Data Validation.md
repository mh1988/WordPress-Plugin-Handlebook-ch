#### 数据验证

通常将数据验证用于外部来的数据上，例如用户输入的数据，或者通过API获得的外部API获得的数据

数据验证例子：

- 检测是否含有空白
- 检查用户输入的手机号是否正确
- 检测输入的邮编的有效性
- 检测一个数量的字段是否大于0

**应该在程序进行其它操作之前就做好数据验证**

> 比较好的方式是在前端用 javascript 进行数据验证之后在后端用php再做一次验证

#### 验证数据

有至少三种方式：PHP内置函数、WordPress核心函数、自定义函数

##### PHP内置函数

一些课用于基础验证的PHP内置函数：(这里不翻译了，详情看PHP手册)

- `isset()` and `empty()` for checking whether a variable exists and isn’t blank
- `mb_strlen()` or `strlen()` for checking that a string has the expected number of characters
- `preg_match()`, `strpos()` for checking for occurrences of certain strings in other strings
- `count()` for checking how many items are in an array
- `in_array()` for checking whether something exists in an array

##### WordPress核心函数

WordPress提供一些有用的函数可以帮助你验证不同类型的数据，一些🌰：

- `is_email()` 验证邮箱是否一个有效的邮箱.
- `term_exists()` checks whether a tag, category, or other taxonomy term exists.
- `username_exists()` 检测用户名是否存在
- `validate_file()` 验证输入的文件路径是否是一个真实的路径（不是验证这个文件是否存在）

这里  [WordPress code reference](https://developer.wordpress.org/reference/) 有很多类似的函数.
搜索类似这种名字: `*_exists()`, `*_validate()`, and `is_*()`. 但并不是所有这类都是验证函数, 但是很多可以帮助到你

#####自定义PHP函数和javscript函数

你可以自己写php、javascript函数引入到你的插件中。你写的函数名字最好是像一个问题，类似这样(is_phone, is_avaliable, is_us_zipcode).

这个函数应该根据数据是否有效返回一个boolean， true或false



##### Example 1

Let’s say you have an U.S. zip code input field that a user submits.

```php
<input id="wporg_zip_code" type="text" maxlength="10" name="wporg_zip_code">
```

本字段最多允许输入10个字符，没有限制输入字符类型，用户可能输入 `1234567890` 或者其他无效的字符类似 `eval()`.

 `maxlength` 属性仅会在浏览器中起作，所以你仍然需要在服务器上对长度进行验证，否则攻击者可能会修改`maxlength` 的值.

验证美国邮政编码的函数:

```php
function is_us_zip_code($zip_code)
{
    // scenario 1: empty
    if (empty($zip_code)) {
        return false;
    }
 
    // scenario 2: more than 10 characters
    if (strlen(trim($zip_code)) > 10) {
        return false;
    }
 
    // scenario 3: incorrect format
    if (!preg_match('/^\d{5}(\-?\d{4})?$/', $zip_code)) {
        return false;
    }
 
    // passed successfully
    return true;
}
```

当你处理表单的时候，你可以根据返回值进行下一步操作:

```php
if (isset($_POST['wporg_zip_code']) && is_us_zip_code($_POST['wporg_zip_code'])) {
    // your action
}
```

通过使用内置的PHP函数将其与一个允许的排序键数组进行比较`in_array()`,这可以防止用户传递恶意数据，并可能危害网站。

在检查数组之前，使用`[`sanitize_key`](https://codex.wordpress.org/Function_Reference/sanitize_key)`,这个wordpress内置函数可以确保，字符串为小写（因为`in_array`	区分大小写）

 [`in_array`](https://php.net/in_array) 的第三个参数，为`ture`开启严格检验模式，这将不仅校验值是否一样还校验类型是否一致，会验证输入的key是否一个字符串或者其它的数据类型

```php
$allowed_keys = ['author', 'post_author', 'date', 'post_date'];
 
$orderby = sanitize_key($_POST['orderby']);
 
if (in_array($orderby, $allowed_keys, true)) {
    // modify the query to sort by the orderby key
}
```

