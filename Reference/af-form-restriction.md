# `af/form/restriction`

Restrict a form based on custom conditions. Return false to display form normally or return a message which should be displayed instead of the form fields.

The first conditional in the example should always be included in order to not override other restrictions

```php
<?php

function restrict_form( $restriction, $form, $args ) {
	// Added in case another restriction already applies
	if ( $restriction ) {
	    return $restriction;
	}
	
	if ( condition_to_hide_form ) {
	    return 'This message will be displayed instead of the form';
	}
	
	return false;
}
add_filter( 'af/form/restriction/key=FORM_KEY', 'restrict_form', 10, 3 );
```

## Modifiers

- `af/form/restriction` — Applies to all forms.
- `af/form/restriction/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/restriction/id=FORM_ID` — Applies to forms with a specific post ID.
