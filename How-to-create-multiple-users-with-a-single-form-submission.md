# How to create multiple users with a single form submission

When you need to create more than one user from a single form submission — for example, registering a primary user along with their family members, or inviting a list of collaborators in one go — Advanced Forms Pro's built-in user-creation UI won't cover it (that one's a single user per submission). The fix is a repeater field holding one row per new user, and a custom submit handler that iterates the rows and inserts each user via WordPress core.

## Set up the form

Add a repeater field to your field group with sub-fields for each piece of user data you need. At minimum, you'll want first name, last name, and email — add any other fields your site requires.

On the form itself, make sure **Enable user editing?** is disabled. You're handling user creation in PHP, so AF's built-in user step would just get in the way.

## Create the users on submission

Hook into [`af/form/submission`](https://advancedforms.github.io/actions/af/form/submission/), read the repeater with `af_get_field()`, and call [`wp_insert_user()`](https://developer.wordpress.org/reference/functions/wp_insert_user/) for each row:

```php
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // Get the new-user rows from the repeater field.
    $new_users = af_get_field( 'new_users' );

    if ( empty( $new_users ) ) {
        return;
    }

    // Insert one user per repeater row.
    foreach ( $new_users as $new_user ) {

        $user_id = wp_insert_user( [
            'role'       => 'subscriber',
            'user_pass'  => wp_generate_password(),
            'first_name' => $new_user['first_name'],
            'last_name'  => $new_user['last_name'],
            'user_email' => $new_user['email'],
            'user_login' => $new_user['email'], // Required — the user won't be created without it.
        ] );

        if ( is_wp_error( $user_id ) ) {
            // Handle the error here if you need to — log it, surface a notice, etc.
            continue;
        }

        // Send the standard new-user emails to both the user and the site admin.
        wp_send_new_user_notifications( $user_id, 'both' );
    }

}, 10, 3 );
```

A few things worth knowing:

- `user_login` is mandatory. Using the email address is the simplest approach when you don't want users picking their own username.
- `wp_generate_password()` gives each user a strong random password. The new-user notification email includes a password-reset link so they can set their own.
- `wp_send_new_user_notifications( $user_id, 'both' )` sends WordPress's standard new-user emails — one to the user with their login details, and one to the site admin. Pass `'user'` or `'admin'` if you only want one of them.
- Wrap the `is_wp_error()` check around `$user_id` before doing anything else with it — `wp_insert_user()` returns a `WP_Error` on failure (most commonly a duplicate email), and continuing past that will throw.

## See also

- [How to redirect the form submission after user creation](./redirect-after-user-creation/)
- [How to set the newly created user as author of a newly created post](./set-new-user-as-post-author/)
