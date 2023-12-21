# Working with hidden fields

todo - intro

## Rendering hidden fields

You may use the `af/form/hidden_fields` action do render hidden fields in a form.

```php
add_action( 'af/form/hidden_fields', function( $form, $args ) {
	// Restrict to a specific form
	if( $form['key'] !== 'form_62bd15508b9c9' ){
		return;
	}
	?>
	<input type="hidden" name="my_hidden_field_1" value="Value 1" />
	<input type="hidden" name="my_hidden_field_2" value="Value 2" />
	<?php
}, 10, 2 );

// Use the form key
add_action( 'af/form/hidden_fields/key=form_62bd15508b9c9', 'callback', 10, 2 );

// Use the form post ID
add_action( 'af/form/hidden_fields/id=1234', 'callback', 10, 2 );
```

## Processing hidden fields

todo - document this

## Related docs

- [Pass arbitrary values through to a form](Pass-arbitrary-values-through-to-a-form.md)