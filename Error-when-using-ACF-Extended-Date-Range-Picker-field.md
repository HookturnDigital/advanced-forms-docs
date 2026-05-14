# Error when using ACF Extended 'Date Range Picker' field

> **Third-party note:** This article describes a known issue with **ACF Extended**'s Date Range Picker field, a third-party plugin's add-on. The behaviour and fix shown here were correct at the time of writing — third-party plugins may change their APIs, hooks, or fields over time, so verify against the current ACF Extended documentation if the workaround stops working.

**Symptom:** Submitting a form that contains an ACF Extended **Date Range Picker** (`acfe_date_range_picker`) field throws one of the following PHP errors, pointing at `field-date-range-picker.php` inside `acf-extended-pro`:

```
PHP Warning: Illegal string offset 'start' in /path/to/acf-extended-pro/pro/includes/fields/field-date-range-picker.php
```

or, on newer PHP versions:

```
Fatal error: Uncaught TypeError: Cannot access offset of type string on string in /path/to/acf-extended-pro/pro/includes/fields/field-date-range-picker.php
```

**Cause:** As part of submission, Advanced Forms runs every submitted value through ACF's `acf_format_value()` so that the formatted version can be used in email notifications, success messages, redirects, and so on. ACF Extended's Date Range Picker field expects its raw value to be an array (with `start` / `end` keys) when its format handler runs, but in some cases it receives a plain string and then tries to access array offsets on it — which is what triggers the warning/fatal.

The same crash is reproducible inside ACF Extended's own forms when the formatted value is rendered in an ACFE success message, so it looks like a bug in the field type's format handler rather than something specific to Advanced Forms.

**Fix:** Short-circuit ACF's format pipeline for this specific field type whenever the value is already a string, so the broken offset access never runs. Drop the following into your theme's `functions.php` or a small mu-plugin:

```php
add_filter( 'acf/pre_format_value', function ( $null, $value, $post_id, $field ) {

    if ( $field['type'] === 'acfe_date_range_picker' && is_string( $value ) ) {
        return $value;
    }

    return $null;

}, 10, 4 );
```

Returning a non-null value from `acf/pre_format_value` tells ACF to skip its normal format handler for that field, which in turn skips ACF Extended's broken handler. Non-string values fall through to ACF Extended as before, so legitimate array values still format normally.
