# Adding custom email headers (CC, BCC, Reply-To) to notification emails

Use the [`af/form/email/headers`](../af-form-email-headers/) filter to add custom headers to a notification email — `Reply-To`, `CC`, `BCC`, or anything else `wp_mail()` accepts.

The filter has a form-key suffix variant (`af/form/email/headers/key=FORM_KEY`) which is the cleanest way to target a specific form:

```php
add_filter( 'af/form/email/headers/key=YOUR_FORM_KEY_HERE', function ( $headers, $email, $form, $fields ) {

    // Reply-To
    $headers[] = 'Reply-To: support@example.com';

    // CC — comma-separated for multiple recipients.
    $headers[] = 'CC: alex@example.com, jane@example.com';

    // BCC.
    $headers[] = 'BCC: archive@example.com';

    return $headers;

}, 10, 4 );
```

Each header is a single string in standard `Name: value` format. To set multiple recipients on a single header, comma-separate them within the value (don't push multiple `CC:` headers).

If you'd rather check the form key inside the callback than use the suffix variant, hook the unsuffixed filter and gate on `$form['key']`:

```php
add_filter( 'af/form/email/headers', function ( $headers, $email, $form, $fields ) {
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $headers;
    }

    $headers[] = 'Reply-To: support@example.com';

    return $headers;

}, 10, 4 );
```
