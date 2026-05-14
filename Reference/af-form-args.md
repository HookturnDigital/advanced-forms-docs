# `af/form/args`

Alter the arguments used to display a form. The arguments are either passed to the function call or defined as attributes on a shortcode.

```php
<?php

function filter_args( $args, $form ) {
    $args['submit_text'] = 'Send';
    return $args;
}
add_filter( 'af/form/args/key=FORM_KEY', 'filter_args', 10, 2 );
```

## Modifiers

- `af/form/args` — Applies to all forms.
- `af/form/args/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/args/id=FORM_ID` — Applies to forms with a specific post ID.
