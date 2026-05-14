# `af/form/after_fields`

Triggered after the submit button.

```php
<?php

function after_fields( $form, $args ) {
    echo 'After fields';
}
add_action( 'af/form/after_fields/key=FORM_KEY', 'after_fields', 10 ,2 );
```

## Modifiers

- `af/form/after_fields` — Applies to all forms.
- `af/form/after_fields/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/after_fields/id=FORM_ID` — Applies to forms with a specific post ID.
