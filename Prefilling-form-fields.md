# Prefilling form fields

You may choose to prefill form fields on form render using a variety of methods.

## Available methods

### Shortcode argument

Due to the potential complexity of field values and shortcodes lacking support for complex data structures, it is not
currently possible to prefill values via the shortcode. Instead, it is best to use the `advanced_form()` function or the
`af/field/prefill_value` filter as documented below.

### Function argument

When using the `advanced_form()` function, you may pass a `values` argument to the function. This argument must be an
array of key/value pairs where the key is the field name (or field key) and the value is the value to prefill the field
with. For example:

```php
advanced_form( 'form_62bd15508b9c9', [
	'values' => [
		// These are mapping values to field names
		'name' => 'Jon Snow',
		'email_address' => 'john@example.com',
		// This is mapping a value to a field key
		'field_62cd2f54f09b1' => 'Stark',
	]
] );
```

### Using a filter

You may also use the `af/field/prefill_value` filter to prefill values. The filter runs shortly before a field is
rendered and provides access to any current value, the field object, the form object, and the form render args so it is
possible to make decisions about what value to prefill based on the current state of the form. For example:

```php
add_filter( 'af/field/prefill_value', function( $value, $field, $form, $args ) {

	// Limit handling to a specific form
	if ( $form['key'] !== 'form_62bd15508b9c9' ) {
		return $value;
	}

	if ( $field['name'] === 'name' ) {
		$value = 'Jon Snow';
	}
	
	if ( $field['key'] === 'field_62cd2f54f09b1' ) {
		$value = 'Stark';
	}
	
	return $value;
}, 10, 4 );
```

In the above example, any fields of the same name will be pre-filled with the value `Jon Snow` so the one filter could
be used across multiple forms if needed. The filter also provides specific variants to make it easier to target fields
by name or by field key. The functions hooked to these variants will only run for fields matching the specified name or
key.

```php
add_filter( "af/field/prefill_value/name=$field_name", 'your_prefill_callback', 10, 4 );
add_filter( "af/field/prefill_value/key=$field_key", 'your_prefill_callback', 10, 4 );
```

## Prefilling complex ACF fields

Some ACF fields have particular values structures to adhere to. Use the following as a guide to pre-filling various
complex field types.

### Prefilling repeater fields

You may prefill repeater fields by mapping an array of rows to the field's name or key. Sub fields must use the field
key as the array key – if you attempt to use the sub field's name, the field's default value will be used.

The following example demonstrates how to prefill a repeater field when using the `advanced_form()` function:

```php
advanced_form( 'form_62bd15508b9c9', [
	'values' => [
		// 'books' is the field name but you may also use the field key
		'books' => [
			[
				'field_64a640c6a53f1' => 'A Game of Thrones',
				'field_64a640cea53f2' => '1996',
			],
			[
				'field_64a640c6a53f1' => 'A Clash of Kings',
				'field_64a640cea53f2' => '1998',
			]
		],
	]
] );
```

The following example demonstrates how to prefill a repeater field when using the `af/field/prefill_value` filter:

```php
add_filter( 'af/field/prefill_value', function( $value, $field, $form, $args ) {
	if( $field['name'] === 'books' ) {
		$value = [
			[
				'field_64a640c6a53f1' => 'A Game of Thrones',
				'field_64a640cea53f2' => '1996',
			],
			[
				'field_64a640c6a53f1' => 'A Clash of Kings',
				'field_64a640cea53f2' => '1998',
			]
		];
	}

	return $value;
}, 10, 4);
```

### Prefilling group fields

You may prefill group fields by mapping an array of sub field values to the field's name or key. Just like repeaters,
sub fields must use the field key as the array key.

The following example demonstrates how to prefill a group field when using the `advanced_form()` function:

```php
advanced_form( 'form_62bd15508b9c9', [
	'values' => [
		// 'house' is the field name but you may also use the field key
		'house' => [
			'field_64a6434dacb0a' => 'Stark',
			'field_64a64363acb0b' => 'Winterfell',
		],
	]
] );
```

The following example demonstrates how to prefill a group field when using the `af/field/prefill_value` filter:

```php
add_filter( 'af/field/prefill_value', function( $value, $field, $form, $args ) {
	if( $field['name'] === 'house' ) {
		$value = [
			'field_64a6434dacb0a' => 'Stark',
			'field_64a64363acb0b' => 'Winterfell',
		];
	}

	return $value;
}, 10, 4);
```

### Prefilling flexible content fields

You may prefill flexible content fields by mapping an array of layouts to the field's name or key. Just like repeaters,
layout sub fields must use the field key as the array key. Additionally, each layout array must have an `acf_fc_layout`
key specifying which layout the array represents.

The following example demonstrates how to prefill a flexible content field when using the `advanced_form()` function:

```php
advanced_form( 'form_62bd15508b9c9', [
	'values' => [
		// 'life_events' is the field name but you may also use the field key
		'life_events' => [
			[
				'acf_fc_layout' => 'joined',
				// Joined
				'field_64a64546a1030' => "Night's Watch",
				// Position
				'field_64a645b0a1035' => 'Steward',
			],
			[
				'acf_fc_layout' => 'appointed',
				// Title
				'field_64a645d2a1038' => 'Lord Commander',
				// By
				'field_64a64621a1039' => 'Election',
			],
			[
				'acf_fc_layout' => 'appointed',
				// Title
				'field_64a645d2a1038' => 'King in the North',
				// By
				'field_64a64621a1039' => 'Election',
			],
		]
	]
] );
```

The following example demonstrates how to prefill a flexible content field when using the `af/field/prefill_value`
filter:

```php
add_filter( 'af/field/prefill_value', function ( $value, $field, $form, $args ) {
	if ( $field['name'] === 'life_events' ) {
		$value = [
			[
				'acf_fc_layout' => 'joined',
				'field_64a64546a1030' => "Night's Watch",
				'field_64a645b0a1035' => 'Steward',
			],
			[
				'acf_fc_layout' => 'appointed',
				'field_64a645d2a1038' => 'Lord Commander',
				'field_64a64621a1039' => 'Election',
			],
			[
				'acf_fc_layout' => 'appointed',
				'field_64a645d2a1038' => 'King in the North',
				'field_64a64621a1039' => 'Election',
			],
		];
	}

	return $value;
}, 10, 4 );
```

### Prefilling clone fields

You may prefill clone fields by mapping an array of clone values to the field's name or key. If you wish to use the
field name, be mindful that this changes depending on the clone field's **Prefix Field Names** setting. If the clone
field is set to prefix field names, you must use a composite field name. e.g;

```php
$clone_field_name = 'cloned';
$cloned_field_name = 'name';
$composite_name = $clone_field_name . '_' . $cloned_field_name;

advanced_form( 'form_62bd15508b9c9', [
	'values' => [
		$composite_name => 'Jon Snow',
	]
] );
```

If the clone field is set to **not** prefix field names, you may use the cloned field's name as though it were a field
directly in the field group. e.g;

```php
advanced_form( 'form_62bd15508b9c9', [
	'values' => [
		'name' => 'Jon Snow',
	]
] );
```

When using field keys, you must use a composite of both the clone and cloned field keys joined with an underscore
regardless of the field's **Prefix Field Names** setting. e.g;

```php
$clone_field_key = 'field_64a64a0b0a103a';
$cloned_field_key = 'field_64a64a1e0a103b';
$composite_key = $clone_field_key . '_' . $cloned_field_key;

advanced_form( 'form_62bd15508b9c9', [
	'values' => [
		$composite_key => 'Jon Snow',
	]
] );
```

## How to pre-fill fields from values in a URL query string

Given prefilling is done via PHP, you may prefill fields from any source you wish. One common source is the URL query
string. This is useful when you wish to prefill fields from a link in an email or from a link on another page. It's
important to note that the URL query string is not secure and should not be used to prefill sensitive data

[htdocs_highlight]For security reasons, it is a good idea to [sanitize](https://developer.wordpress.org/apis/security/sanitizing/) and
[validate](https://developer.wordpress.org/apis/security/data-validation/) (where possible) any values you prefill from
the URL query string.[/htdocs_highlight]

```php
add_filter( 'af/field/prefill_value', function ( $value, $field, $form, $args ) {
	if ( $field['name'] === 'name' && isset( $_GET['prefill_name'] ) ) {

		// We're expecting a string here. Sanitize the value for security.
		$name = sanitize_text_field( $_GET['prefill_name'] );
		 
		// For this field, we're only accepting letters, spaces, and hyphens. Remove any characters that don't match.
		$name = preg_replace( '/[^a-zA-Z\s-]/', '', $name );

		$value = $name;
	}

	return $value;
}, 10, 4 );
```