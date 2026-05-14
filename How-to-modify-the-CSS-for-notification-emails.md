# How to modify the CSS for notification emails

Use the [`af/form/email/styles`](https://advancedforms.github.io/filters/af/form/email/styles/) filter to append (or replace) the inline CSS that styles a notification email. The filter returns a CSS string that gets embedded in the email's `<head>`.

The `/key=FORM_KEY` variant targets a specific form without needing a key check in the callback:

```php
add_filter( 'af/form/email/styles/key=YOUR_FORM_KEY_HERE', function ( $css ) {

    $additional = '
        table {
            width: 1000px;
        }
        td {
            padding: 8px 12px;
        }
    ';

    return $css . $additional;

} );
```

To replace the default styles entirely rather than append, return your own CSS without concatenating `$css`:

```php
add_filter( 'af/form/email/styles/key=YOUR_FORM_KEY_HERE', function () {
    return '
        body { font-family: -apple-system, sans-serif; }
        table { width: 100%; border-collapse: collapse; }
    ';
} );
```

## Email-client caveats

Email clients vary wildly in CSS support — Outlook in particular drops many modern properties and renders some things very differently from a web browser. Stick to inline-friendly properties (table-based layout, web-safe fonts, `cellpadding`/`cellspacing` for spacing) for reliable rendering across clients. Litmus and Mailtrap are good ways to preview your email rendering before relying on it.
