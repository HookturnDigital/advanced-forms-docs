# How to exclude sub-fields from rendering in a form

Advanced Forms' [`exclude_fields`](https://advancedforms.github.io/guides/available-arguments/#exclude_fields) arg only applies to top-level fields — sub-fields inside groups, repeaters, and flexible-content fields aren't evaluated against it. If you need to exclude sub-fields, the snippet below extends the behaviour recursively by patching ACF's `load_fields` during form rendering.

```php
<?php
/**
 * Advanced Forms — exclude sub-fields support
 * Drop into a custom plugin or your theme's functions.php. The mu-plugins
 * directory is also a fine home for it.
 */

defined( 'WPINC' ) || die();

add_action( 'af/form/before_fields', function ( $form, $args ) {
    global $afesfm;
    $afesfm['args'] = $args;

    add_filter( 'acf/load_fields', 'afesfm_modify_fields_array' );
}, 10, 2 );

add_action( 'af/form/after_fields', function () {
    remove_filter( 'acf/load_fields', 'afesfm_modify_fields_array' );
}, 10, 2 );

function afesfm_modify_fields_array( $fields ) {
    return array_filter( $fields, function ( &$field ) {
        global $afesfm;

        $excludes = $afesfm['args']['exclude_fields'] ?? [];
        if ( empty( $excludes ) ) {
            return true;
        }

        // Top-level: drop the field outright if its name or key is excluded.
        if ( in_array( $field['key'], $excludes, true ) || in_array( $field['name'], $excludes, true ) ) {
            return false;
        }

        // Sub-fields: recurse so groups/repeaters/flex get the same treatment.
        if ( isset( $field['sub_fields'] ) && is_array( $field['sub_fields'] ) ) {
            $field['sub_fields'] = afesfm_modify_fields_array( $field['sub_fields'] );
        }

        return true;
    } );
}
```

With the snippet in place, sub-field names and keys are honoured by `exclude_fields` the same way top-level fields already are.
