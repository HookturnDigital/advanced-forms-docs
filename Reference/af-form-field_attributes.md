# `af/form/field_attributes`

Filter attributes on field wrappers. Use to add classes, set an ID, or add new attributes.

`$attributes` is an array of HTML attributes and their values.

```php
<?php

function filter_field_attributes( $attributes, $field, $form, $args ) {
    $attributes['id'] = 'form-id';
    
    return $attributes;
}
add_filter( 'af/form/field_attributes/key=FORM_KEY', 'filter_field_attributes', 10, 4 );
```

## Modifiers

- `af/form/field_attributes` — Applies to all forms.
- `af/form/field_attributes/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/field_attributes/id=FORM_ID` — Applies to forms with a specific post ID.
