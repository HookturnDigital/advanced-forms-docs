# `af/form/email/content`

Filter the contents of a form email before sending.

```php
<?php

function filter_email_content( $content, $email, $form, $fields ) {
	// Add some extra text to the end of the content
    $content .= ' Some extra content';
    
    return $content;
}
add_filter( 'af/form/email/content/key=FORM_KEY', 'filter_email_content', 10, 4 );
```

## Modifiers

- `af/form/email/content` — Applies to all forms.
- `af/form/email/content/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/email/content/id=FORM_ID` — Applies to forms with a specific post ID.
