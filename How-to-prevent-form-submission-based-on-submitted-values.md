# How to prevent form submission based on submitted values

To block a submission based on what was entered — without raising a hard PHP error — read the submitted value in [`af/form/before_submission`](../af-form-before_submission/) and call `af_add_submission_error()` to surface a user-facing error.

```php
add_action( 'af/form/before_submission', function ( $form, $fields, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    $value = af_get_field( 'YOUR_FIELD_NAME_HERE' );

    if ( $value === 'some unwanted text' ) {
        af_add_submission_error( 'This is a custom error message.' );
    }

}, 10, 3 );
```

When `af_add_submission_error()` is called, Advanced Forms aborts the submission and renders the error message above the form — exactly the same UX as a standard validation failure. The user can fix the field and submit again.

You can call `af_add_submission_error()` multiple times to surface multiple errors at once.

## See also

- [How to enforce a unique field value for each form entry](./enforce-unique-field-value/) — the same pattern, applied to checking for duplicate entries.
