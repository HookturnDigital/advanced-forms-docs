# `af/form/before_title`

Triggered at the beginning of a form, before the title.

```php
<?php

function before_title( $form, $args ) {
    echo 'Before title';
}
add_action( 'af/form/before_title/key=FORM_KEY', 'before_title', 10, 2 );
```


## Modifiers

- `af/form/before_title` — Applies to all forms.
- `af/form/before_title/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/before_title/id=FORM_ID` — Applies to forms with a specific post ID.
