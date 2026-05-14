# `af/form/editing/user_created`

:::caution[Pro feature]
This hook is only available in Advanced Forms Pro.
:::

Triggered after a user has been created. Not triggered when a user is edited.

```php
<?php

function form_user_created( $user, $form, $args ) {
    // Do something with the created user.
    // $user is a WP_User object.
}
add_action( 'af/form/editing/user_created/key=FORM_KEY', 'form_user_created', 10, 3 );
```

## Modifiers

- `af/form/editing/user_created` — Applies to all forms.
- `af/form/editing/user_created/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/editing/user_created/id=FORM_ID` — Applies to forms with a specific post ID.
