# How to send email notifications to the current logged-in user

To send a notification to whoever's logged in at submission time — without hard-coding any email addresses — override the recipient in [`af/form/email/recipient`](../af-form-email-recipient/):

```php
add_filter( 'af/form/email/recipient', function ( $recipient, $email, $form, $fields ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $recipient;
    }

    $user = wp_get_current_user();

    if ( $user && $user->ID ) {
        return $user->user_email;
    }

    return $recipient;

}, 10, 4 );
```

This pattern works when the form is gated behind a login. If the form can be submitted by anonymous visitors too, the `$user->ID` guard sends the original recipient list as a fallback — so the notification still goes somewhere useful.

## See also

- [How to send notification emails to multiple recipients](./send-emails-to-multiple-recipients/) — extend this pattern to send to both the user and an internal address.
