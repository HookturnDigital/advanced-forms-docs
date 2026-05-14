# `af/form/success_message`

Alter the success message before it's shown. Submitted field values can be retrieved using `af_get_field( 'FIELD_NAME' )`.

```php
<?php

function change_success_message( $success_message, $form, $args ) {
    return 'New success message';
}
add_filter( 'af/form/success_message/key=FORM_KEY', 'change_success_message', 10, 3 );
```

## Modifiers

- `af/form/success_message` — Applies to all forms.
- `af/form/success_message/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/success_message/id=FORM_ID` — Applies to forms with a specific post ID.
