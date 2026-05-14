# How to pre-fill form field values

There are two ways to pre-fill form field values: via the `values` form arg (best for static data known at render time) or via the [`af/field/prefill_value`](https://advancedforms.github.io/filters/af/field/prefill_value/) filter (best when the value needs to be computed or conditional).

## Pre-filling via the `values` form arg

When rendering the form with `advanced_form()`, pass a `values` array keyed by field name:

```php
advanced_form( 'YOUR_FORM_KEY_HERE', [
    'values' => [
        'first_name' => 'John',
        'last_name'  => 'Doe',
    ],
] );
```

The [`values`](https://advancedforms.github.io/guides/available-arguments/#values) arg works with the function only — the `[advanced_form ...]` shortcode doesn't currently support it.

## Pre-filling via `af/field/prefill_value`

For programmatic / conditional pre-fills, hook the filter:

```php
add_filter( 'af/field/prefill_value', function ( $value, $field, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $value;
    }

    if ( $field['name'] === 'first_name' ) {
        return 'John';
    }

    if ( $field['name'] === 'last_name' ) {
        return 'Doe';
    }

    return $value;

}, 10, 4 );
```

The filter fires per-field on render, so you can pull values from anywhere — `wp_get_current_user()`, a related post, an API call, the URL — and return them per `$field['name']` (or `$field['key']` if you prefer keys).

## See also

- [How to pre-fill fields from values in a URL query string](./prefill-from-url-query/)
- [How to pre-fill group fields](./prefill-group-fields/)
- [How to pre-fill clone fields](./prefill-clone-fields/)
