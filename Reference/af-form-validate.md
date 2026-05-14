# `af/form/validate`

Triggered before a form is submitted, giving a chance to perform extra validation. Validation errors should be added using `af_add_error( $field_name_or_key, $message )`.

```php
<?php

function validate_form( $form, $args ) {
    $age = af_get_field( 'age' );
    if ( $age > 18 ) {
        af_add_error( 'age', 'You must be above 18' );
    }
}
add_action( 'af/form/validate/key=FORM_KEY', 'validate_form', 10, 2 );
```

## Modifiers

- `af/form/validate` — Applies to all forms.
- `af/form/validate/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/validate/id=FORM_ID` — Applies to forms with a specific post ID.
