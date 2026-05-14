# How to edit the post content & post title

To map a form field to a core post field (`post_title` or `post_content`):

1. Add a field for the value you want to capture (a text field for the title, a wysiwyg or textarea for the content).
2. In the form's settings, open the **Editing** tab and set the field under **Post title** or **Post content**.

When the form is submitted, Advanced Forms writes the value from the chosen ACF field into the matching core post field.

Because these fields don't usually need to render in the admin (the admin already has `post_title` / `post_content` fields built in), [hide them from the admin](./how-to-hide-fields-from-the-admin-but-show-them-on-a-form/) so they only show in the form context.

## Programmatic alternative

If you need more control — e.g. transforming the value or deriving the title from multiple fields — use the [`af/form/editing/post_data`](../af-form-editing-post_data/) filter:

```php
add_filter( 'af/form/editing/post_data', function ( $post_data, $form, $args ) {

    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $post_data;
    }

    $post_data['post_title']   = af_get_field( 'your_title_field' );
    $post_data['post_content'] = af_get_field( 'your_content_field' );

    return $post_data;

}, 10, 3 );
```

The filter applies on both create and edit submissions.
