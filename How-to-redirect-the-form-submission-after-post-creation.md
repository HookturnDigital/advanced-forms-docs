# How to redirect the form submission after post creation

When using Advanced Forms Pro to create posts, you'll often want to redirect to a custom URL — and pass the new post's ID through to that URL. Hook into [`af/form/editing/post_created`](../af-form-editing-post_created/) and override the redirect on the submission:

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

## Redirect to the post's edit screen instead

Same pattern, different target — handy when your form is a "create a new entry" page and you want the user dropped straight into the editing context once the post exists. Swap the redirect URL for the post's edit link:

```php
add_action( 'af/form/editing/post_created', function ( $post, $form, $args ) {

    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // Send the user to the WP admin edit screen for the new post.
    AF()->submission['args']['redirect'] = get_edit_post_link( $post->ID, 'url' );

}, 10, 3 );
```

`get_edit_post_link()` returns the correct admin URL for the post type and respects the current user's edit capability. Pass `'url'` as the second argument so you get a raw URL rather than the HTML-escaped version meant for output.

If you're redirecting to a front-end edit page instead (e.g. a separate form on a public page that loads the post for editing), build the URL the same way as the parent example — `add_query_arg( 'post', $post->ID, home_url( '/your-edit-page/' ) )` — and assign it to `AF()->submission['args']['redirect']`.
