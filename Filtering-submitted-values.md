# Filtering submitted values

If you need to modify a submitted field value before it is processed, you may use the `af/form/submission/value` filter.
This filter is applied to the raw input value of a field before it is formatted by ACF.

[htdocs_highlight]

Note, this filter is only available in version 1.9.3.4 and later.

[/htdocs_highlight]

```php
add_filter( 'af/form/submission/value', function ( $value, $field, $form, $args ) {
	// Restrict to a specific form
	if ( $form['key'] !== 'form_62bd15508b9c9' ) {
		return $value;
	}

	// Modify a value by field name
	if ( $field['name'] === 'first_name' ) {
		$value = ucfirst( $value );
	}

	// Modify a value by field key
	if ( $field['key'] === 'field_5e5e5e5e5e5e5' ) {
		$value = trim( $value );
	}

	return $value;
}, 10, 4 );
```

## Filtering sub field values

Sub field values, such as those within a repeater, group, of flexible content field, do not individually pass through
this filter. Instead, the parent field value is passed through the filter. If you need to modify sub field values, you
will need to loop through the `$value` variable and apply your modifications to each sub field value.

## Timing and related caveats

The `af/form/submission/value` filter fires before validation. Any custom validation rules added using
the `af/form/validate` filter the access field values via the `af_get_field()` function will be affected by this filter.

Note that Advanced Forms validates fields in the `AF()->submission['fields']` array whereas ACF core validates fields in
the `$_POST['acf']` array. This means that ACF's core validation will not be affected by the `af/form/submission/value`
filter.

Validation is fired via AJAX so a modified value won't show in the form, even though it affects validation.

### Custom validation conflicts

If you are using the `af/form/validate` action to enforce a specific value, and the `af/form/submission/value` filter is
used to modify the value to something that is invalid, the validation will fail regardless of the user input. e.g;

```php
add_filter( 'af/form/submission/value', function ( $value, $field, $form, $args ) {
	// Modify the value to something that is invalid
	if ( $field['name'] === 'first_name' ) {
		$value = 'bar';
	}

	return $value;
}, 10, 4 );

add_action( 'af/form/validate', function ( $form, $args ) {
	// Enforce a specific value
	if ( af_get_field( 'first_name' ) !== 'foo' ) {
		af_add_error( 'first_name', 'First name should be "foo"' );
	}
}, 10, 4 );
```

### Modfying values based on other fields

This filter is executed before the Advanced Forms submission array is created. This means that it is not possible to use
the `af_get_field()` function to get the value of another submitted field. Instead, you can access the raw values using
their field keys from the `$_POST['acf']` array. e.g;

```php
add_filter( 'af/form/submission/value', function ( $value, $field, $form, $args ) {
	// Restrict to a specific form
	if ( $form['key'] !== 'form_62bd15508b9c9' ) {
		return $value;
	}

	// Check for the field we want to modify
	if ( $field['name'] === 'first_name' ) {

		// Only modify the value if another field value meets a condition
		if ( $_POST['acf']['field_5e5e5e5e5e5e5'] === 'foo' ) {
			$value = 'bar';
		}
	}

	return $value;
}, 10, 4 );
```