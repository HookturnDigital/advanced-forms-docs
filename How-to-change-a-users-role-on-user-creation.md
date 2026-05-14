# How to conditionally modify the user role on user creation

When using a form to create users, you can adjust the role based on what was submitted using the [`af/form/editing/user_data`](../af-form-editing-user_data/) filter — and `af_get_field()` to read the submitted values:

```php
add_filter( 'af/form/editing/user_data', function ( $user_data, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $user_data;
    }

    // Pick a role based on a submitted value.
    if ( af_get_field( 'some_submitted_field' ) === 'some value' ) {
        $user_data['role'] = 'author';
    }

    return $user_data;

}, 10, 3 );
```

See also: [How to change a user's role on post creation](./change-user-role-post-creation/) for the post-creation equivalent.
