# How to use a form to manage ACF fields on an options page

Saving form values into an ACF options page isn't a built-in feature, but two short hooks make it work.

## Saving field values to options

When the same field group is used for both the form and the options page, this is enough to mirror submissions to the options:

```php
// Save every submitted field on this form to the 'options' object.
add_action( 'af/form/submission', function ( $form, $fields, $args ) {
    if ( $form['key'] === 'YOUR_FORM_KEY_HERE' ) {
        af_save_all_fields( 'options' );
    }
}, 10, 3 );
```

## Loading field values from options

To make the form a full editing experience, you'll also want it to pre-fill from the values currently saved to options:

```php
// Pre-fill form fields from the 'options' object on render.
add_filter( 'af/field/prefill_value', function ( $value, $field, $form, $args ) {
    if ( $form['key'] === 'YOUR_FORM_KEY_HERE' ) {
        return get_field( $field['key'], 'options' );
    }
    return $value;
}, 10, 4 );
```
