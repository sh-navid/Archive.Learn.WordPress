# Learn-WordPress
## Plugin
- Make `my-plugin` folder in `wp-content/plugins`
- Make a `index.php` file in `my-plugin` and fill these contents
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

    add_filter( 'preprocess_comment' , 'my_func' );


    function my_func( $commentdata ) {
        $commentdata['comment_content'] = strtoupper( $commentdata['comment_content'] );
        return $commentdata;
    }
    ~~~
- Go to `Plugins` tab on wordpress and activate this plugin
## Short Code
## [Hook](https://developer.wordpress.org/reference/hooks/)
- [Action](https://developer.wordpress.org/plugins/hooks/)
- [Filter](https://developer.wordpress.org/plugins/hooks/)
## Meta Data
## Custom Post Type
## Taxonomy
## Cron