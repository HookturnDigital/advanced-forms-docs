# How to dynamically change a form's success message when a post is updated

To show a different success message when a form updates an existing post (vs. creating a new one), use two hooks: stash the updated post's ID on `AF()->submission` from the `post_updated` hook, then read it back in the success-message filter.

```php
// Capture the updated post ID during submission so we can read it later.
add_action( 'af/form/editing/post_updated', function ( $post, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // Prefix custom keys (e.g. xyz_) to avoid clashes with future core fields.
    AF()->submission['xyz_updated_post_id'] = $post->ID;

}, 10, 3 );

// If the custom key is set, override the success message.
add_filter( 'af/form/success_message', function ( $success_message, $form, $args ) {

    if ( empty( AF()->submission['xyz_updated_post_id'] ) ) {
        return $success_message;
    }

    $post_id = AF()->submission['xyz_updated_post_id'];
    return "Post $post_id updated.";

}, 10, 3 );
```

This works because `AF()->submission` persists across the lifetime of a single submission — anything you stash in `post_updated` is still there when `success_message` runs.
