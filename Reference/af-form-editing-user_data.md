# `af/form/editing/user_data`

:::caution[Pro feature]
This hook is only available in Advanced Forms Pro.
:::

Override the user data used when a user is created or updated. User data is the `$userdata` argument passed to [`wp_insert_user`](https://developer.wordpress.org/reference/functions/wp_insert_user/).

```php
<?php

function modify_user_data( $user_data, $form, $args ) {
    // Override user role
    $user_data['role'] = 'administrator';

    return $user_data;
}
add_filter( 'af/form/editing/user_data/key=FORM_KEY', 'modify_user_data', 10, 3 );
```

## Modifiers

- `af/form/editing/user_data` — Applies to all forms.
- `af/form/editing/user_data/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/editing/user_data/id=FORM_ID` — Applies to forms with a specific post ID.
