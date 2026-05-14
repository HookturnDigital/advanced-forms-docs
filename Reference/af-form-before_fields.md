# `af/form/before_fields`

Triggered right before the fields and after the description.

```php
<?php

function before_fields( $form, $args ) {
    echo 'Before fields and after description';
}
add_action( 'af/form/before_fields/key=FORM_KEY', 'before_fields', 10, 2 );
```

## Modifiers

- `af/form/before_fields` — Applies to all forms.
- `af/form/before_fields/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/before_fields/id=FORM_ID` — Applies to forms with a specific post ID.
