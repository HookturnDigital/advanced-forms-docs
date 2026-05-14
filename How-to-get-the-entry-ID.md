# How to get the entry ID

Form entries are stored as a custom post type under the hood. When entry storage is enabled on a form, the created entry's post ID is available on the submission object under the `entry` key:

```php
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    $entry_id = AF()->submission['entry'] ?? null;

    if ( $entry_id ) {
        // Use the entry ID — e.g. attach extra meta, fire an integration, etc.
    }

}, 10, 3 );
```

`AF()->submission['entry']` is `null` when entry storage isn't enabled on the form (or on submission types that don't generate an entry), so the null-coalesce + guard above is the safe pattern.

## See also

- [How to store IP addresses on form entries](./store-ip-addresses-on-entries/) — common downstream use of the entry ID.
- [How to display form entries](./display-form-entries/) — querying stored entries elsewhere on the site.
