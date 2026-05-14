# `af/form/email/styles`

Alter the CSS used for email notifications.

```php
<?php

function filter_email_styles( $styles, $email, $form ) {
  // Remove all default styles
  return '';
}
add_filter( 'af/form/email/styles/key=FORM_KEY', 'filter_email_styles', 10, 3 );
```

## Modifiers

- `af/form/email/styles` — Applies to all forms.
- `af/form/email/styles/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/email/styles/id=FORM_ID` — Applies to forms with a specific post ID.
