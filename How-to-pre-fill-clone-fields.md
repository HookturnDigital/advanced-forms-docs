# How to pre-fill clone fields

Pre-filling a clone field is a bit fiddly because the field name (and key) that appears at render time depends on the clone's **Prefix Field Names** setting. When it's enabled, the rendered field's name is the clone's name *joined* with the cloned field's name.

Handle both cases inside [`af/field/prefill_value`](../af-field-prefill_value/):

```php
add_filter( 'af/field/prefill_value', function ( $value, $field, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $value;
    }

    $clone  = 'YOUR_CLONE_FIELD_NAME_HERE';
    $cloned = 'YOUR_CLONED_FIELD_NAME_HERE';

    // Prefix Field Names ENABLED → the rendered field's name is "<clone>_<cloned>".
    if ( $field['name'] === "{$clone}_{$cloned}" ) {
        return 'Value 1';
    }

    // Prefix Field Names DISABLED → the rendered field's name is just <cloned>.
    if ( $field['name'] === $cloned ) {
        return 'Value 2';
    }

    return $value;

}, 10, 4 );
```

The same logic applies if you'd rather match on field keys — prefixed keys look like `field_abc123_field_def456`.

## See also

- [How to pre-fill form field values](./pre-fill-form-field-values/)
- [How to pre-fill group fields](./prefill-group-fields/)
