# `af/form/email/headers`

Filter the headers of a form email before sending. `$headers` should be an array of email headers.

```php
<?php

function filter_email_headers( $headers, $email, $form, $fields ) {
	// Set the reply-to address
	$headers[] = 'Reply-To: john@doe.com';

	return $headers;
}
add_filter( 'af/form/email/headers/key=FORM_KEY', 'filter_email_headers', 10, 4 );
```

## Modifiers

- `af/form/email/headers` — Applies to all forms.
- `af/form/email/headers/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/email/headers/id=FORM_ID` — Applies to forms with a specific post ID.
