# `af/field/prefill_value`

Prefill field values before displaying form. Can be used for example to provide dynamic default values.

```php
<?php

function prefill_form_field( $value, $field, $form, $args ) {
    return 'Pre-filled value';
}
add_filter( 'af/field/prefill_value/key=FIELD_KEY', 'prefill_form_field', 10, 4 );
```

## Modifiers

- `af/field/prefill_value` — Applies to all fields.
- `af/field/prefill_value/name=FIELD_NAME` — Applies to fields with a specific name.
- `af/field/prefill_value/key=FIELD_KEY` — Applies to fields with a specific key.
