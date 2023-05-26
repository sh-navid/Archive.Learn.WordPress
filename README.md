# Learn-WordPress
## [Hook](https://developer.wordpress.org/reference/hooks/)
- [Action](https://developer.wordpress.org/plugins/hooks/)
- [Filter](https://developer.wordpress.org/plugins/hooks/)
## Plugin
- Make `my-plugin` folder in `wp-content/plugins`
- Make a `index.php` file in `my-plugin` and fill these contents
    - ~~~ php
        <?php
        /**
        * Plugin Name: my-plugin
        * Plugin URI: https://www.my-site.com/
        * Description: My Plugin
        * Version: 0.1
        * Author: my-name
        * Author URI: https://www.my-site.com/
        **/

        add_filter( 'preprocess_comment' , 'my_func' );

        function my_func( $commentdata ) {
            $commentdata['comment_content'] = strtoupper( $commentdata['comment_content'] );
            return $commentdata;
        }
     ~~~
- Go to `Plugins` tab on wordpress and activate this plugin
- [Plugin Menu](https://developer.wordpress.org/plugins/administration-menus/top-level-menus/)
    - ~~~php
        add_menu_page(
            string $page_title,
            string $menu_title,
            string $capability,
            string $menu_slug,
            callable $function = '',
            string $icon_url = '',
            int $position = null
        );
    ~~~
## [ShortCode](https://developer.wordpress.org/plugins/shortcodes/)
- You can use it inside posts, pages, and widgets
- You can also register this shortcode inside plugin
    - ~~~ php
        add_shortcode('myshortcode', 'my_short_code');

        function my_short_code( $atts = [], $content = null) {
            $content="<h1>My Shortcode Content</h1>";
            return $content;
        }
    ~~~
- Then you can put `[myshortcode]` inside for example post content
## Meta Data
## Custom Post Type
## Taxonomy
## Cron
## Plugin example
**Disclaimer**: this is a bad practice and its not secure; its just for testing
- `index.php`
~~~ php
<?php
/**
 * Plugin Name: my-plugin
 * Plugin URI: https://www.my-site.com/
 * Description: My Plugin
 * Version: 0.1
 * Author: my-name
 * Author URI: https://www.my-site.com/
 **/


function get_local_file_contents($file_path)
{
    ob_start();
    include $file_path;
    $contents = ob_get_clean();
    return $contents;
}


add_filter('preprocess_comment', 'my_func');
function my_func($commentdata)
{
    if (get_local_file_contents("options.txt") == "lower") {
        $commentdata['comment_content'] = strtolower($commentdata['comment_content']);
    } else {
        $commentdata['comment_content'] = strtoupper($commentdata['comment_content']);
    }
    return $commentdata;
}


add_shortcode('myshortcode', 'my_short_code');
function my_short_code($atts = [], $content = null)
{
    $content = "<h1>My Shortcode Content</h1>";
    return $content;
}


function myplugin_page()
{
    echo "Option: ";
    echo get_local_file_contents("options.txt");


    echo "<h1>" . esc_html(get_admin_page_title()) . "</h1>";
    echo '<form action="' . esc_attr(plugin_dir_url(__FILE__)) . 'save.php?type=upper" method="post">';
    submit_button(__('Save Upper'));
    echo '</form>';
    echo '<form action="' . esc_attr(plugin_dir_url(__FILE__)) . 'save.php?type=lower" method="post">';
    submit_button(__('Save Lower'));
    echo '</form>';
}


add_action('admin_menu', 'myplugin_options_page');
function myplugin_options_page()
{
    add_menu_page(
        'My Plugin', // Page Title
        'My Plugin Options', // Menu title
        'manage_options', // Capability
        'myplugin', // Menu Slug
        'myplugin_page', // Function
        plugin_dir_url(__FILE__) . 'icon.png', // Icon
        20 // Position
    );
}
~~~
- `save.php`
~~~php
<?php
$myfile = fopen("options.txt", "w") or die("Unable to open file!");
fwrite($myfile, $_GET["type"]);
fclose($myfile);

header("Location: http://localhost:8000/wp-admin/admin.php?page=myplugin");
exit();
~~~