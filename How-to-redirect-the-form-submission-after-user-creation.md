# How to redirect the form submission after user creation

When using Advanced Forms Pro to create users, you'll often want to redirect to a custom URL — and pass the new user's ID through to that URL. Hook into [`af/form/editing/user_created`](https://advancedforms.github.io/actions/af/form/editing/user_created/) and override the redirect on the submission:

```php
add_action( 'af/form/editing/user_created', function ( $user, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // Build the URL using the new user's ID and apply it to the submission's
    // redirect arg — Advanced Forms will use this in place of the form's
    // configured redirect.
    AF()->submission['args']['redirect'] = 'https://example.com/welcome?user_id=' . $user->ID;

}, 10, 3 );
```

See also: [How to redirect the form submission after post creation](./redirect-after-post-creation/).
