# How to update a user's password from a form field

To update a user's password from an Advanced Forms field:

1. Add a password field to your form so the user can enter the new password.
2. Hook into [`af/form/editing/user_updated`](https://advancedforms.github.io/actions/af/form/editing/user_updated/) to read the submitted value with `af_get_field()`.
3. Call [`wp_set_password()`](https://developer.wordpress.org/reference/functions/wp_set_password/) to apply the new password.

```php
add_action( 'af/form/editing/user_updated', function ( $user, $form, $args ) {

    // Only run for a specific form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // Read the submitted password.
    $password = af_get_field( 'password_field' );

    if ( ! $password ) {
        return;
    }

    // Apply the new password. Note: this logs the user out.
    wp_set_password( $password, $user->ID );

    // Sign the user back in immediately so they don't lose their session.
    wp_set_auth_cookie( $user->ID );

}, 10, 3 );
```
