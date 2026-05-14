# `af/form/email/subject`

Filter the subject line of a form email before sending.

```php
<?php

function filter_email_subject( $subject, $email, $form, $fields ) {
  // Alter the subject line
  $subject = 'New subject';
    
  return $subject;
}
add_filter( 'af/form/email/subject/key=FORM_KEY', 'filter_email_subject', 10, 4 );
```

## Modifiers

- `af/form/email/subject` — Applies to all forms.
- `af/form/email/subject/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/email/subject/id=FORM_ID` — Applies to forms with a specific post ID.
