# How to set the newly created user as author of a newly created post

When a form creates both a user and a post in the same submission, you'll often want the new user to be the author of the new post. Advanced Forms Pro handles user creation first and stashes the new user's ID on the submission under the `user` key, ready for downstream hooks to use.

Hook into [`af/form/editing/post_data`](../af-form-editing-post_data/) and set `post_author` from the submission:

```php
add_filter( 'af/form/editing/post_data', function ( $post_data, $form, $args ) {

    if ( $form['key'] === 'YOUR_FORM_KEY_HERE' ) {
        $post_data['post_author'] = AF()->submission['user'];
    }

    return $post_data;

}, 10, 3 );
```

The new user is created first, then this filter runs while the post is being assembled — so `AF()->submission['user']` is the user's ID at the moment you need it.
