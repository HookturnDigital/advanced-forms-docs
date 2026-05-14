# `af/form/entry_created`

Triggered after an entry has been created

```php
<?php

function entry_created( $entry_id, $form ) {
    // Do something with entry
}
add_action( 'af/form/entry_created/key=FORM_KEY', 'entry_created', 10, 2 );
```

## Modifiers

- `af/form/entry_created` — Applies to all forms.
- `af/form/entry_created/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/entry_created/id=FORM_ID` — Applies to forms with a specific post ID.
