# How to set the post slug (post name) when creating or editing a post

The [`af/form/editing/post_data`](../af-form-editing-post_data/) filter lets you customise any post property — including `post_name` (the URL slug) — before Advanced Forms saves it. The filter applies on both create and edit.

The following snippet derives the slug from the post title, but you can source it from anywhere — a dedicated form field, a meta value, or any other logic:

```php
add_filter( 'af/form/editing/post_data', function ( $post_data, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $post_data;
    }

    // Generate the slug from the post title. Swap this out for a form field
    // value via af_get_field() if you'd rather the user set it explicitly.
    $post_data['post_name'] = sanitize_title( $post_data['post_title'] );

    return $post_data;

}, 10, 3 );
```

Always run the value through `sanitize_title()` so it's a valid URL slug — WordPress will otherwise reject or mangle it.
