# How to change the value of a field during submission based on another value

There isn't a dedicated filter for modifying field values mid-submission yet, so the workaround is to modify the `$_POST` superglobal directly *before* Advanced Forms' submission handler runs.

Hook into `init` at a priority lower than 10 (Advanced Forms' default) so the modification lands first:

```php
add_action( 'init', function () {

    // Bail unless we're processing an Advanced Forms submission.
    if ( ! isset( $_POST['af_form'] ) ) {
        return;
    }

    // Only run for the target form.
    if ( $_POST['af_form'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    $field_1_key = 'field_63603df414f2a';
    $field_2_key = 'field_63603e7514f2b';

    // If field 1's submitted value matches a condition, override field 2's value.
    if (
        isset( $_POST['acf'][ $field_1_key ] )
        && $_POST['acf'][ $field_1_key ] === 'some value'
    ) {
        $_POST['acf'][ $field_2_key ] = 'field 2 updated value';
    }

}, 5 );
```

Replace the form key and field keys with your own.
