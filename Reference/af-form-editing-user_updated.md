# `af/form/editing/user_updated`

:::caution[Pro feature]
This hook is only available in Advanced Forms Pro.
:::

Triggered after a user has been edited. Not triggered when a user is created.

```php
<?php

function form_user_updated( $user, $form, $args ) {
    // Do something with the updated user.
    // $user is a WP_User object.
}
add_action( 'af/form/editing/user_updated/key=FORM_KEY', 'form_user_updated', 10, 3 );
```

## Modifiers

- `af/form/editing/user_updated` — Applies to all forms.
- `af/form/editing/user_updated/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/editing/user_updated/id=FORM_ID` — Applies to forms with a specific post ID.
