# How to send notification emails to multiple recipients

A single Advanced Forms notification can be sent to any number of recipients. The simplest way is in the UI:

1. Open the form's **Notifications** settings.
2. In the notification's **Custom recipient** field, enter the addresses comma-separated:
   `alex@example.com, jane@example.com, support@example.com`

## Overriding the recipient list in code

For dynamic recipient lists — e.g. based on form values or the current user — use the [`af/form/email/recipient`](../af-form-email-recipient/) filter:

```php
add_filter( 'af/form/email/recipient', function ( $recipient, $email, $form, $fields ) {

    // Target a specific form + specific notification (by name from the UI).
    if (
        $form['key'] === 'YOUR_FORM_KEY_HERE'
        && $email['name'] === 'My Example Notification'
    ) {
        return 'alex@example.com, jane@example.com';
    }

    return $recipient;

}, 10, 4 );
```

You can return either a comma-separated string or an array — both work with the underlying `wp_mail()` call.

## See also

- [Adding custom email headers (CC, BCC, Reply-To)](./custom-email-headers/) — useful when you want one primary recipient with cc/bcc copies.
