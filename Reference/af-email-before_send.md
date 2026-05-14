# `af/email/before_send`

Triggered before an email is sent.

```php
<?php

function before_email_send( $email, $form ) {
    // Do something with email
}
add_action( 'af/email/before_send/key=FORM_KEY', 'before_email_send', 10, 2 );
```

## Modifiers

- `af/email/before_send` — Applies to all forms.
- `af/email/before_send/key=FORM_KEY` — Applies to forms with a specific key.
- `af/email/before_send/id=FORM_ID` — Applies to forms with a specific post ID.
