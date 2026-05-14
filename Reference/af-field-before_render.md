# `af/field/before_render`

Modify an ACF field before it's rendered. Similar to `acf/prepare_field`.

```php
<?php

function modify_field( $field, $form, $args ) {
    $field['label'] = 'New field label';
    return $field;
}
add_filter( 'af/field/before_render/name=FIELD_NAME', 'modify_field', 10, 3 );
```

## Modifiers

- `af/field/before_render` — Applies to all fields.
- `af/field/before_render/name=FIELD_NAME` — Applies to fields with a specific name.
- `af/field/before_render/key=FIELD_KEY` — Applies to fields with a specific key.
