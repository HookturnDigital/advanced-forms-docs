# How to change a user's role on post creation

To change the post author's user role after a form creates a post, hook into [`af/form/editing/post_created`](../af-form-editing-post_created/) and call `wp_update_user()`:

```php
add_action( 'af/form/editing/post_created', function ( $post, $form, $args ) {

    // Optionally limit to a specific form.
    // if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
    //     return;
    // }

    // Add any guard conditions you need here so the role only changes for the
    // right users (e.g. check current role, check submitted field values).

    wp_update_user( [
        'ID'   => $post->post_author,
        'role' => 'editor',
    ] );

}, 10, 3 );
```

See also: [How to conditionally modify the user role on user creation](./change-user-role-user-creation/) for the user-creation equivalent.
