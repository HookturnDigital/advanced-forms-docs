# Customizing how the form renders

Advanced Forms offers a vast array of options, hooks, and filters for customizing how a form renders.

## Excluding fields from the form

You may wish to exclude certain fields from the form. This can be done using
the [`exclude_fields` display argument](Display-arguments.md#excludefields).

If you wish to do so using PHP, you may use the `af/form/args` filter to modify the display args:

```php
add_filter( 'af/form/args', function( $args, $form ) {
	// Restrict to a specific form
	if( $form['key'] !== 'form_62bd15508b9c9' ){
		return $args;
	}
	
	$args['exclude_fields'] = [
		'my_field',
		'my_second_field',
		'field_5f5b2b2b2b2b2', // Exclude a group by its field key
	];

	return $args;
}, 10, 2 );

// You may also use more specific filters to target a form either by its post ID or form key
add_filter( "af/form/args/id=$form_id", ... );
add_filter( "af/form/args/key=$form_key", ... );
```

## Excluding sub fields

Sub fields, such as those within groups, repeaters, or flexible content fields, are not currently evaluated for
exclusion. This is a known limitation that will be addressed in a future release. For now, there is a workaround which
is available in our support documents:

[How to exclude sub fields from rendering in a form](https://hookturn.freshdesk.com/support/solutions/articles/44002397391)

## Controlling the form title and description

### Setting the title and description

You may set the form title and description on the form edit screen as follows:

![advanced-forms-pro-for-acf-form-title-and-description-fields.jpg](images/advanced-forms-pro-for-acf-form-title-and-description-fields.jpg)
advanced-forms-pro-for-acf-form-title-and-description-fields.jpg

If you wish to do so using PHP, you may use the `af/form/before_render` filter to modify the form object:

```php
add_filter( 'af/form/before_render', function( $form, $args ) {
	// Restrict to a specific form
	if( $form['key'] !== 'form_62bd15508b9c9' ){
		return;
	}
	
	$form['title'] = 'My custom form title';
	$form['display']['description'] = 'My custom form description message.';
	
	return $form;
}, 10, 2 );

// You may also use more specific filters to target a form either by its post ID or form key
add_filter( "af/form/before_render/id=$form_id", ... );
add_filter( "af/form/before_render/key=$form_key", ... );
```

### Hiding the title and description

When rendering a form, you may toggle the display of the title and description using the
[`display_title` and `display_description` display arguments](Display-arguments.md#displaytitle).

For programmatic control, see [filtering display arguments](Display-arguments.md#filtering-display-arguments).

## Customizing the submit button text

todo
You can pass custom submit button text to the [`submit_text` display argument](Display-arguments.md#submittext) when
displaying a form.

## Customizing the form wrapper attributes

todo - document `af/form/attributes` filter

### Add custom CSS classes for CSS styling

todo

## Adding HTML before and after a form

todo - document the `af/form/enqueue` and `af/form/after` filters

## Adding HTML before the form title

todo - document `af/form/before_title` filter. Note this renders before submission errors.

## Adding hidden fields

See [Working with hidden fields](Working-with-hidden-fields.md).

## Adding HTML before and after all form fields

todo - document `af/form/before_field_wrapper` and `af/form/after_field_wrapper` hooks.
todo - document `af/form/before_fields` and `af/form/after_fields` hooks.

## Adding HTML before and after field groups

todo - document `af/field_group/before_field_group` and `af/field_group/after_field_group` hooks.

## Adding HTML before and after fields

todo - document the `af/field/before_field` and `af/field/after_field` hooks.

## Customizing the form success message

todo - document the `af/form/success_message` filter

## Prefilling form fields

See [Prefilling form fields](Prefilling-form-fields.md).

## Modifying the ACF field array before render

todo - document the `af/field/before_render` filter

## Modifying the field wrapper attributes

todo - document the `af/form/field_attributes`

### Add custom CSS classes for CSS styling

todo

## Controlling the field instruction placement

todo - mention the form arg
todo - document the `af/field/instruction_placement` filter

## Customise the form restricted output

See [Restricting access to a form](Restricting-access-to-a-form.md).

## Related docs

- [Display arguments](Display-arguments.md)
- [Working with hidden fields](Working-with-hidden-fields.md)
- [Restricting access to a form](Restricting-access-to-a-form.md)
- [Prefilling form fields](Prefilling-form-fields.md)
- [How to exclude sub fields from rendering in a form](https://hookturn.freshdesk.com/support/solutions/articles/44002397391)