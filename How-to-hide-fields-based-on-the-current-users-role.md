# How to hide fields based on the current user's role

To prevent a field from rendering based on the current user's role, return `false` from the [`af/field/before_render`](../af-field-before_render/) filter.

```php
add_filter( 'af/field/before_render', function ( $field, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $field;
    }

    $current_user = wp_get_current_user();
    if ( ! $current_user ) {
        return $field;
    }

    // Hide a couple of fields from non-administrators.
    if ( ! in_array( 'administrator', $current_user->roles, true ) ) {
        $admin_only_keys = [ 'field_63603df414f2a', 'field_63603e7514f2b' ];
        if ( in_array( $field['key'], $admin_only_keys, true ) ) {
            return false;
        }
    }

    return $field;

}, 10, 3 );
```

Returning `false` stops the field from rendering entirely — it isn't hidden via CSS, so the field never appears in the DOM. Submitting users can't bypass the hide by tweaking the form in dev tools.

If you want hidden-in-form-but-still-saved behaviour instead, that's a different pattern — see [How to hide fields from the admin but show them on a form](./how-to-hide-fields-from-the-admin-but-show-them-on-a-form/) for the inverse case.
