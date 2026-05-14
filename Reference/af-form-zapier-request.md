# `af/form/zapier/request`

:::caution[Pro feature]
This hook is only available in Advanced Forms Pro.
:::

Change the Zapier API request before it's sent. 

```php
<?php

function modify_zapier_request( $request, $form, $args ) {
  // Combine two fields and send to Zapier with key "full_name"
  $full_name = af_get_field( 'first_name' ) . ' ' . af_get_field( 'last_name' );
  $request['body']['full_name'] = $full_name;

  return $request;
}
add_filter( 'af/form/zapier/request/key=FORM_KEY', 'modify_zapier_request', 10, 3 );
```

## Modifiers

- `af/form/zapier/request` — Applies to all forms.
- `af/form/zapier/request/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/zapier/request/id=FORM_ID` — Applies to forms with a specific post ID.
