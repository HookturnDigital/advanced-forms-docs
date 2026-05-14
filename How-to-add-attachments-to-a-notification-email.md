# How to add attachments to a notification email

Use the [`af/form/email/attachments`](https://advancedforms.github.io/filters/af/form/email/attachments/) filter to add file paths to a notification email's attachments array. Each entry is an absolute server path; the file is attached via `wp_mail()`'s standard attachment mechanism.

```php
add_filter( 'af/form/email/attachments', function ( $attachments, $email, $form, $fields ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $attachments;
    }

    // Append an absolute server path to a file you want attached.
    $attachments[] = WP_CONTENT_DIR . '/uploads/terms.pdf';

    return $attachments;

}, 10, 4 );
```

## Attaching an uploaded form field

When attaching a file the user uploaded via the form itself, read the file field with `af_get_field()` and resolve its path:

```php
add_filter( 'af/form/email/attachments', function ( $attachments, $email, $form, $fields ) {

    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $attachments;
    }

    $file = af_get_field( 'your_file_field' );

    if ( ! empty( $file ) ) {
        // ACF file fields can return ID, URL, or array depending on Return Format.
        // For an attachment ID:
        $path = get_attached_file( is_array( $file ) ? $file['ID'] : $file );

        if ( $path ) {
            $attachments[] = $path;
        }
    }

    return $attachments;

}, 10, 4 );
```

Adjust the `$file` handling based on your file field's **Return Format** setting (Array / URL / ID).
