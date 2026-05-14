# `af/field/calculate_value`

:::caution[Pro feature]
This hook is only available in Advanced Forms Pro.
:::

Provide your own value to be displayed in a calculated field. `af_get_field` can be used as normal but beware that no validation is performed on fields.

```php
<?php

function calculate_field_value( $value, $field, $form, $args ) {
    $quantity = af_get_field( 'quantity' );
    $price = 10;
    $total = $quantity * $price;

    return '$' . $total;
}
add_filter( 'af/field/calculate_value/name=FIELD_NAME', 'calculate_field_value', 10, 4 );
```

## Modifiers

- `af/field/calculate_value` — Applies to all fields.
- `af/field/calculate_value/name=FIELD_NAME` — Applies to fields with a specific name.
- `af/field/calculate_value/key=FIELD_KEY` — Applies to fields with a specific key.
