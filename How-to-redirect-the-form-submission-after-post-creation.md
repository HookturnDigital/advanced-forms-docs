# How to redirect the form submission after post creation

When using Advanced Forms Pro to create posts, you'll often want to redirect to a custom URL — and pass the new post's ID through to that URL. Hook into [`af/form/editing/post_created`](https://advancedforms.github.io/actions/af/form/editing/post_created/) and override the redirect on the submission:

```php
add_action( 'af/form/editing/post_created', function ( $post, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // Build the URL using the new post's ID and apply it to the submission's
    // redirect arg — Advanced Forms will use this in place of the form's
    // configured redirect.
    AF()->submission['args']['redirect'] = 'https://example.com?post=' . $post->ID;

}, 10, 3 );
```

If you'd prefer to redirect to the created post's permalink directly, use `get_permalink( $post->ID )` in place of the hard-coded URL.
