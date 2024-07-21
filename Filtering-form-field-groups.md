# Filtering form field groups

If you need to modify the array of field groups for a given form, you may use the `af/form/field_groups` filter. This
filter is applied to the array of field groups after they are queried from ACF and before they are handled by Advanced
Forms. This filter makes it possible to:

1. Modify the order of field groups
2. Add or remove field groups
3. Modify fields within a field group

[htdocs_highlight]

Note, this filter is only available in version 1.9.3.5 and later.

[/htdocs_highlight]

```php
add_filter( 'af/form/field_groups', function ( $field_groups, $form_key ) {
	// Restrict to a specific form
	if ( $form_key !== 'form_64c0bb30bd954' ) {
		return $field_groups;
	}

	// Reverse order of field groups
	$field_groups = array_reverse( $field_groups );

	return $field_groups;
}, 10, 2 );
```

You may also use a variation of this filter that targets a specific field group by key:

```php
add_filter( 'af/form/field_groups/key=form_64c0bb30bd954', function ( $field_groups, $form_key ) {
	// Reverse order of field groups
	$field_groups = array_reverse( $field_groups );

	return $field_groups;
}, 10, 2 );
```

## Caveats

When using this filter, avoid calling the `af_get_form()` function as this results in a recursive error in some cases.

This filter modifies the array of field groups across more than just the form rendering context – it will affect the
form edit screen, success messages, email notifications, and any other context where field groups are queried for the
form.