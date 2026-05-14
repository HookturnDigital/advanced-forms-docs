# Field values are ending up with slashes in front of certain characters

**Symptom:** Values saved by a form come back out with backslashes in front of quotes, apostrophes, or other special characters. For example, `that's` is stored or displayed as `that\'s`, and `"quoted"` becomes `\"quoted\"`. The extra characters accumulate if the value is saved more than once.

**Cause:** WordPress slashes incoming request data on the way in (a legacy holdover from `magic_quotes`). ACF normally handles unslashing during its own update path, but in some contexts — custom save handlers, values written from non-standard request flows, or content read back through filters that haven't run unslash — the slashes survive into the rendered value.

**Fix:** Hook into ACF's value formatting and strip the slashes on read. This runs every time a field's value is fetched via `get_field()` / `the_field()`, so the underlying data is left alone but the output is clean.

```php
add_filter( 'acf/format_value/type=text', function ( $value ) {
    return is_string( $value ) ? stripslashes( $value ) : $value;
}, 10, 1 );
```

If textareas are affected as well, add the same filter for that field type:

```php
add_filter( 'acf/format_value/type=textarea', function ( $value ) {
    return is_string( $value ) ? stripslashes( $value ) : $value;
}, 10, 1 );
```

If you're saving form data manually (e.g. via `af/form/submission` or a custom handler that reads `$_POST` directly), the better fix is to unslash the data *before* it's stored:

```php
add_action( 'af/form/submission/key=YOUR_FORM_KEY_HERE', function ( $form, $fields, $args ) {
    $raw = wp_unslash( $_POST['acf'] ?? [] );
    // …then use $raw instead of $_POST['acf'].
}, 10, 3 );
```

That way the value goes into the database clean and you don't need the read-time `stripslashes()` filter at all.
