# How to append new rows to an existing repeater field

When a form edits a post or user and the underlying repeater is large, mapping the field directly means the form has to render every existing row — that gets unfriendly quickly. Paginated repeaters (ACF 6.0+) aren't currently available on front-end forms, so the workaround is to *not* map the repeater at all, and instead append new rows to the existing repeater via a custom submission handler. The form renders an empty repeater; the rows the user adds get appended to whatever's already on the record.

## Step 1 — Remove the repeater from the form's field mapping

Open the form's settings and make sure the repeater field is *not* mapped. We'll be saving the rows manually.

## Step 2 — Append rows in a custom submission handler

```php
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    // Optionally limit to a specific form.
    // if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
    //     return;
    // }

    // Read the submitted rows.
    $items = af_get_field( 'items' );

    if ( ! $items ) {
        return;
    }

    foreach ( $items as $item ) {

        // The target ID is the post ID when editing a post:
        $id = AF()->submission['args']['post'];

        // …or the user ID when editing a user (note the user_ prefix that ACF expects):
        // $id = 'user_' . AF()->submission['args']['user'];

        add_row( 'items', $item, $id );
    }

}, 10, 3 );
```

Replace `'items'` with the field name (or key) of your repeater on both sides — the call to `af_get_field()` reads the submitted rows, and `add_row()` writes them to the existing record's repeater.
