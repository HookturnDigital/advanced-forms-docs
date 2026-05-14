# How to pre-fill group fields

Group fields expect their pre-fill value as an array keyed by sub-field key (not name). Return that shape from [`af/field/prefill_value`](../af-field-prefill_value/) when the field being rendered is your group:

```php
add_filter( 'af/field/prefill_value', function ( $value, $field, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $value;
    }

    if ( $field['name'] === 'YOUR_GROUP_FIELD_NAME_HERE' ) {
        return [
            'field_12345678' => 'Sub field 1 value',
            'field_23456789' => 'Sub field 2 value',
        ];
    }

    return $value;

}, 10, 4 );
```

The keys are the ACF field keys of the sub-fields inside the group — you can copy them from the ACF field editor. Using keys (rather than sub-field names) is what makes this format work; names won't be picked up.

## See also

- [How to pre-fill form field values](./pre-fill-form-field-values/) — overview and the simpler `values` arg approach.
- [How to pre-fill clone fields](./prefill-clone-fields/)
