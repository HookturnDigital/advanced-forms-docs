# Store a calculated field value

[Calculated fields](../working-with-calculated-fields/) are display-only by design — the value isn't stored against the ACF object on submission. To persist the result, store it manually from `af/form/submission` into a regular ACF field.

```php
// 1. Compute the calculated field's value when the form renders.
add_filter( 'af/field/calculate_value/name=calculated_1', function () {
    return af_get_field( 'number_1' ) + af_get_field( 'number_2' );
}, 10, 0 );

// 2. On submission, read the calculated value and store it against the
//    created/edited record.
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // Pick the right ACF object ID for the record this form is editing:
    //   Post → AF()->submission['post']
    //   User → 'user_' . AF()->submission['user']
    $acf_object_id = AF()->submission['post'];

    $calculated = af_get_field( 'calculated_1' );

    update_field( 'stored_total', $calculated, $acf_object_id );

}, 20, 3 ); // priority 20 — after Advanced Forms' built-in post/user editing handler runs.
```

Replace `calculated_1` and `stored_total` with the field names from your form/field group, and adjust the object-ID logic for whether the form is editing a post, a user, an options page, or something else.
