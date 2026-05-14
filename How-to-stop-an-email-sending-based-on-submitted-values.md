# How to stop an email sending based on submitted values

To conditionally suppress an email notification based on what was submitted, return `false` from [`af/form/email/recipient`](../af-form-email-recipient/) when the condition isn't met. A `false` return tells Advanced Forms to skip the email.

```php
add_filter( 'af/form/email/recipient', function ( $recipient, $email, $form, $fields ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $recipient;
    }

    // Suppress the email if a submitted field doesn't match the expected value.
    if ( af_get_field( 'my_field_name' ) !== 'some value' ) {
        return false;
    }

    return $recipient;

}, 10, 4 );
```

If you've got multiple notifications on the form and only want to suppress one of them, also check `$email['name']` (the name set in the **Notifications** UI) so the condition only applies to that notification.

## See also

- [How to prevent emails sending if form is updating a post](./prevent-emails-on-post-update/) — same pattern, gating on `$args['post']` instead of a field value.
