# Dynamically set maximum allowed rows on a repeater field on form render

To set the max-row limit on a repeater dynamically — based on user role, current data, or any other runtime condition — modify the field's settings in [`af/field/before_render`](../af-field-before_render/) before the field renders.

The `name=` variant of the filter is the cleanest way to target a specific repeater:

```php
add_filter( 'af/field/before_render/name=YOUR_REPEATER_FIELD_NAME', function ( $field, $form, $args ) {

    // Replace this with whatever condition you need to gate on.
    $should_limit = true;

    if ( $should_limit ) {
        $field['max'] = 2;
    }

    return $field;

}, 10, 3 );
```

Replace `YOUR_REPEATER_FIELD_NAME` with the ACF field name of the repeater you want to limit, and `$should_limit` with the real condition (e.g. a `wp_get_current_user()` role check, a per-form-args flag, a per-post count).

The `min` and `layout` settings can be overridden the same way — anything that's part of the ACF field config can be patched in `before_render`.
