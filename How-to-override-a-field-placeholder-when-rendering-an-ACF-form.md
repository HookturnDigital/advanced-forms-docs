# How to override a field placeholder when rendering an ACF form

To change a field's placeholder at render time — without editing the field's settings in the ACF UI — modify the field array in [`af/field/before_render`](../af-field-before_render/):

```php
add_filter( 'af/field/before_render', function ( $field, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $field;
    }

    if ( $field['name'] === 'email' ) {
        $field['placeholder'] = 'Enter your email address';
    }

    return $field;

}, 10, 3 );
```

The same pattern works for other field-level settings — `instructions`, `label`, `required`, `default_value`, etc. — anything that's part of the ACF field config can be patched in this filter.

For targeting a single field, use the `name=` variant of the filter to avoid the name-check inside the callback:

```php
add_filter( 'af/field/before_render/name=email', function ( $field, $form, $args ) {
    $field['placeholder'] = 'Enter your email address';
    return $field;
}, 10, 3 );
```
