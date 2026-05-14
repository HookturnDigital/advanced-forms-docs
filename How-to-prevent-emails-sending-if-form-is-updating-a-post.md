# How to prevent emails sending if form is updating a post

To send notification emails only when a form **creates** a new post (not when it edits one), use [`af/form/email/recipient`](../af-form-email-recipient/) to disable the notification on update submissions by returning `false`.

```php
add_filter( 'af/form/email/recipient', function ( $recipient, $email, $form, $fields ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $recipient;
    }

    $args = AF()->submission['args'];

    // Form is configured to create a new post — let the email through.
    if ( $args['post'] === 'new' ) {
        return $recipient;
    }

    // Otherwise, disable the notification.
    return false;

}, 10, 4 );
```
