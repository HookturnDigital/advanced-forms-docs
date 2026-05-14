# `af/form/submission`

Triggered when a form is submitted. Can be used to implement your own submission handling, for example to call third-party APIs. Submitted field values can be retrieved using `af_get_field( 'FIELD_NAME' )`.

More details are available in the [Processing form submissions](../processing-form-submissions/) guide.

```php
<?php

function handle_form_submission( $form, $field, $args ) {
  $email = af_get_field( 'email' );
}
add_action( 'af/form/submission', 'handle_form_submission', 10, 3 );
```

## Modifiers

- `af/form/submission` — Applies to all forms.
- `af/form/submission/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/submission/id=FORM_ID` — Applies to forms with a specific post ID.
