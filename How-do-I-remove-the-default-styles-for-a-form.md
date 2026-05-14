# How do I remove the default styles for a form?

Advanced Forms enqueues three stylesheets when rendering a form — `af-form-style` (Advanced Forms' own) plus `acf-input` and `acf-pro-input` (ACF's). Hook into `af/form/enqueue` and `wp_dequeue_style()` each handle to drop them.

```php
add_action( 'af/form/enqueue', function ( $form, $args ) {

    // Remove Advanced Forms' default style.
    wp_dequeue_style( 'af-form-style' );

    // Remove ACF's default styles.
    wp_dequeue_style( 'acf-input' );
    wp_dequeue_style( 'acf-pro-input' );

}, 10, 2 );
```

To scope this to a single form rather than every form on the site, gate on the form key:

```php
add_action( 'af/form/enqueue', function ( $form, $args ) {

    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    wp_dequeue_style( 'af-form-style' );
    wp_dequeue_style( 'acf-input' );
    wp_dequeue_style( 'acf-pro-input' );

}, 10, 2 );
```

## Tread carefully

Removing these stylesheets is fine when you're styling the form from scratch and want a clean slate. Without them, though, several ACF field types — the WYSIWYG, date/colour pickers, the media uploader UI, repeater layouts — will render unstyled or partially broken until you provide your own CSS. Dequeue when you're ready to own the styling end-to-end, not as a quick cleanup pass.

## See also

- [How to dequeue scripts](./how-to-dequeue-scripts/) — same hook, script counterpart.
