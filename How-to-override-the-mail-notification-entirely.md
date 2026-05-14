# How to override the mail notification entirely

If the built-in notification system doesn't fit — e.g. you want to send via a transactional email service, attach generated PDFs, conditionally pick between templates, or run async — bypass it entirely:

1. Remove every notification from **Form Settings → Notifications**.
2. Send your own email from [`af/form/submission`](https://advancedforms.github.io/actions/af/form/submission/) using `wp_mail()` (or the SDK of whichever email service you use).

```php
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    $to      = af_get_field( 'email' );
    $subject = 'Thanks for your submission';
    $body    = 'Custom message body here…';
    $headers = [ 'Content-Type: text/html; charset=UTF-8' ];

    wp_mail( $to, $subject, $body, $headers );

}, 10, 3 );
```

This gives you full control — and full responsibility. You're no longer benefiting from Advanced Forms' built-in merge-tag resolution or email templating, so build those into your handler as needed.

## When to use the existing filters instead

If you only need to tweak one or two aspects of the built-in notification, the dedicated filters are usually less work:

- [`af/form/email/recipient`](https://advancedforms.github.io/filters/af/form/email/recipient/) — change who receives the email.
- [`af/form/email/headers`](https://advancedforms.github.io/filters/af/form/email/headers/) — add CC/BCC/Reply-To.
- [`af/form/email/styles`](https://advancedforms.github.io/filters/af/form/email/styles/) — modify the email CSS.

Reach for the "send your own" pattern only when those filters aren't enough.
