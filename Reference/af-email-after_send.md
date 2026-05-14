# `af/email/after_send`

Triggered after an email has been sent.

```php
<?php

function after_email_send( $email, $form ) {
    // Do something with email
}
add_action( 'af/email/after_send/key=FORM_KEY', 'after_email_send', 10, 2 );
```

## Modifiers

- `af/email/after_send` — Applies to all forms.
- `af/email/after_send/key=FORM_KEY` — Applies to forms with a specific key.
- `af/email/after_send/id=FORM_ID` — Applies to forms with a specific post ID.
