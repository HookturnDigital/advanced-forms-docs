# How to provide an option to save a post as a draft

To let the user choose whether a form-created post is published immediately or saved as a draft, add a field with explicit options for the post status, then read it in [`af/form/editing/post_data`](https://advancedforms.github.io/filters/af/form/editing/post_data/) to set `post_status` accordingly.

For the example below: add an ACF radio field named `publish_when` with two choices — `now` and `later`.

```php
add_filter( 'af/form/editing/post_data', function ( $post_data, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $post_data;
    }

    $when = af_get_field( 'publish_when' ) ?: 'now';

    $post_data['post_status'] = $when === 'later' ? 'draft' : 'publish';

    return $post_data;

}, 10, 3 );
```

You can extend the same pattern to support more statuses (`pending`, `private`, scheduled `future`) — just map the field options to the [post status](https://wordpress.org/documentation/article/post-status/) values you want.
