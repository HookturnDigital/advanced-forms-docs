# How to enforce a unique field value for each form entry

If you need each form entry to have a unique value for a particular field — e.g. one entry per email address — use the `af/form/before_submission` hook to look for existing entries with the same value, and call `af_add_submission_error()` to block the submission if one already exists.

```php
add_action( 'af/form/before_submission', function ( $form, $fields, $args ) {

    // Configure these for your form.
    $form_key   = 'YOUR_FORM_KEY_HERE';
    $field_name = 'email';

    // Only run for the target form.
    if ( $form['key'] !== $form_key ) {
        return;
    }

    // Read the submitted value we want to enforce uniqueness on.
    $submitted_value = af_get_field( $field_name );

    // Look for any existing entry on this form with the same value.
    $entries = get_posts( [
        'post_type'      => 'af_entry',
        'posts_per_page' => -1,
        'meta_query'     => [
            [
                'key'   => 'entry_form',
                'value' => $form_key,
            ],
            [
                'key'   => $field_name,
                'value' => $submitted_value,
            ],
        ],
    ] );

    // If a match is found, block the submission with a friendly error.
    if ( ! empty( $entries ) ) {
        af_add_submission_error(
            "'$field_name' must be unique. There is already an entry with the value '$submitted_value'."
        );
    }

}, 10, 3 );
```

The form will refuse to submit while a duplicate value is present, and the user will see the error message above the form.
