# How to pre-fill fields from values in a URL query string

To pre-fill a field from a URL query parameter — e.g. `?my-custom-param=some-value` — read `$_GET` inside the [`af/field/prefill_value`](https://advancedforms.github.io/filters/af/field/prefill_value/) filter and return the matching value for the target field.

```php
add_filter( 'af/field/prefill_value', function ( $value, $field, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $value;
    }

    // Match the field by name (or use $field['key'] for the field key), then
    // read the query parameter off the request.
    if (
        $field['name'] === 'YOUR_FIELD_NAME_HERE'
        && isset( $_GET['YOUR_QUERY_PARAM_HERE'] )
    ) {
        return sanitize_text_field( wp_unslash( $_GET['YOUR_QUERY_PARAM_HERE'] ) );
    }

    return $value;

}, 10, 4 );
```

`sanitize_text_field()` covers the common case of a plain-text input — for other field types (textarea, wysiwyg) use the matching sanitiser, and for typed inputs (numbers, dates) cast/validate explicitly.

## Special field types

Some field types need special formatting for their pre-fill value:

- [How to pre-fill group fields](./prefill-group-fields/)
- [How to pre-fill clone fields](./prefill-clone-fields/)
