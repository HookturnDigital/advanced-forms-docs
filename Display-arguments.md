# Display arguments

Display arguments are used to control the way a form is displayed and bahaves. They can be specified when rendering a
form using the shortcode or PHP function. e.g;

`[advanced_form form="form_62bd15508b9c9" submit_text="Send" ajax="1" redirect="/thank-you"]`

Or, if using the PHP function:

```php
advanced_form( 'form_62bd15508b9c9', [
	'submit_text' => 'Send',
	'ajax' => true,
	'values' => [
		'name' => 'John Smith',
		'email' => 'john@example.com',
	],
	'redirect' => '/thank-you',
	...
] );
```

## Available arguments

### submit_text

This controls the the text of the submit button and can be used as follows:

`[advanced_form form="form_62bd15508b9c9" submit_text="Send"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'submit_text' => 'Send' ] );`

The default value is "Submit".

### redirect

This controls the page the user is redirected to after the form is submitted. It can be used as follows:

`[advanced_form form="form_62bd15508b9c9" redirect="/thank-you"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'redirect' => '/thank-you' ] );`

The default value is `null` which will prevent the redirect from occurring.

### ajax

This controls whether the form is submitted via AJAX or not. An AJAX submission occurs without reloading the page and
can provide the user with a faster, more seamless experience. It can be used as follows:

`[advanced_form form="form_62bd15508b9c9" ajax="1"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'ajax' => true ] );`

The default value is false.

### values

This will pre-fill values for specified form fields. Due to the complex structure of this argument, this onlt works when
rendering a form using the PHP function. It can be used as follows:

[//]: # (todo - test this using field keys and document here)
`advanced_form( 'form_62bd15508b9c9', [ 'values' => [ 'field_1_name' => 'value 1', 'field_2_name' => 'value 2' ] ] );`

You may also programmatically pre-fill field values using the `af/field/prefill_value` filter. See
the [Prefilling form fields](Prefilling-form-fields.md) document for more detailed information.

### exclude_fields

This will exclude specified fields from the form. It can be used as follows:

[//]: # (todo - test this using field keys and document here)
`[advanced_form form="form_62bd15508b9c9" exclude_fields="field_1,field_2"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'exclude_fields' => [ 'field_1', 'field_2' ] ] );`

### uploader

This controls which uploader to use for file uploads. You may pass `basic` for a standard file input field or `wp` for
the WordPress media uploader. It can be used as follows:

`[advanced_form form="form_62bd15508b9c9" uploader="wp"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'uploader' => 'wp' ] );`

Be mindful that the WordPress media uploader is not available to users without certain capabilities so the field used
may change depending on the current user. This functionality is core to WordPress and is not controlled by Advanced
Forms.

The default value is `wp` for logged-in users and `basic` for users who do not have the required capabilities.

### filter_mode

When enabled, filter mode will skip the form success message after submission and instead display the form again with
all fields with their submitted values. This is useful when using Advanced Forms as a profile edit screen or similar. It
can be used as follows:

`[advanced_form form="form_62bd15508b9c9" filter_mode="1"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'filter_mode' => true ] );`

The default value is false.

### label_placement

[//]: # (todo - explore options – top, left? – and document this)

### instruction_placement

This controls where the field instructions are displayed. The options are `label` to display the instructions below the
field label or `field` to display the instructions below the field input. It can be used as follows:

`[advanced_form form="form_62bd15508b9c9" instruction_placement="field"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'instruction_placement' => 'field' ] );`

The default value is `label`.

### display_title

This controls whether the form title is displayed or not. It can be used as follows:

`[advanced_form form="form_62bd15508b9c9" display_title="1"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'display_title' => true ] );`

The default value is false.

### display_description

This controls whether the form description is displayed or not. It can be used as follows:

`[advanced_form form="form_62bd15508b9c9" display_description="1"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'display_description' => true ] );`

The default value is false.

### id

This controls the ID attribute of the form element. It can be used as follows:

`[advanced_form form="form_62bd15508b9c9" id="my-custom-form"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'id' => 'my-custom-form' ] );`

The default value is the form key.

### honeypot

This controls whether the honeypot field is displayed or not. The honeypot is a hidden field designed to capture data
from bots to help filter out spam. It can be used as follows:

`[advanced_form form="form_62bd15508b9c9" honeypot="0"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'honeypot' => false ] );`

The default value is true.

### target

This controls the value of the `action` attribute of the form element. This effectively changes the destination the form
is submitted to. It can be used as follows:

`[advanced_form form="form_62bd15508b9c9" target="/some/custom/endpoint"]`

Or, if using the PHP function:

`advanced_form( 'form_62bd15508b9c9', [ 'target' => '/some/custom/endpoint' ] );`

The default value is the current URL as provided by the ACF core function `acf_get_current_url()`.

This should only be used in advanced cases where you need to submit the form to a custom endpoint for a highly
customised form submission process – using the [redirect](#redirect) arg makes sense for most use cases.

### echo

This controls whether the form is echoed or returned when using the PHP function. It can be used as follows:

`$form = advanced_form( 'form_62bd15508b9c9', [ 'echo' => false ] );`

The default value is true.

## Filtering display arguments

You may use the `af/form/args` filter to modify the display arguments before the form is rendered.

```php
add_filter( 'af/form/args', function ( $args, $form ) {
	// Limit changes to a specific form
	if ( $form['key'] === 'form_62bd15508b9c9' ) {
		$args['submit_text'] = 'Send';
		$args['redirect'] = 'http://example.com/thank-you/';
	}

	return $args;
}, 10, 2 );
```

Additionally, you may use a more targeted variation of this filter to modify the display arguments for a specific form
by either the form post ID or form key.

```php
add_filter( 'af/form/args/id=' . $form_post_id, function ( $args, $form ) {
	$args['submit_text'] = 'Send';
	$args['redirect'] = 'http://example.com/thank-you/';

	return $args;
}, 10, 2 );
```

```php
add_filter( 'af/form/args/key=' . $form_key, function ( $args, $form ) {
	$args['submit_text'] = 'Send';
	$args['redirect'] = 'http://example.com/thank-you/';

	return $args;
}, 10, 2 );
```