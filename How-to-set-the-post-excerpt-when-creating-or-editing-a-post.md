# How to set the post excerpt when creating or editing a post

The [`af/form/editing/post_data`](https://advancedforms.github.io/filters/af/form/editing/post_data/) filter lets you customise any post property — including `post_excerpt` — before Advanced Forms saves it. The filter applies on both create and edit.

The following snippet takes the value from a submitted field and assigns it to the new post's excerpt:

```php
add_filter( 'af/form/editing/post_data', function ( $post_data, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $post_data;
    }

    // Pull the value from your form field and assign it to post_excerpt.
    $post_data['post_excerpt'] = af_get_field( 'YOUR_FIELD_NAME_OR_KEY' );

    return $post_data;

}, 10, 3 );
```

The same pattern works for any other field on `$post_data` — `post_title`, `post_content`, `post_status`, `post_author`, etc.
