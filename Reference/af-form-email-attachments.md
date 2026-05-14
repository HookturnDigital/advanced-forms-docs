# `af/form/email/attachments`

Filter the attachments of a form email before sending. `$attachments` should be an array of file paths similar to the attachments parameter for `wp_mail`.

```php
<?php

function filter_email_attachments( $attachments, $email, $form, $fields ) {
  // Add an uploaded file from a file field as an attachemnt
  // The file field should have return format "FileArray"
  $file = af_get_field( 'file' );
  $attachments[] = get_attached_file( $file['id'] );

  return $attachments;
}
add_filter( 'af/form/email/attachments/key=FORM_KEY', 'filter_email_attachments', 10, 4 );
```

## Modifiers

- `af/form/email/attachments` — Applies to all forms.
- `af/form/email/attachments/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/email/attachments/id=FORM_ID` — Applies to forms with a specific post ID.
