# `af/form/mailchimp/request`

:::caution[Pro feature]
This hook is only available in Advanced Forms Pro.
:::

Change the Mailchimp API request before it's sent. The request is for the ["Add member to list" endpoint](https://mailchimp.com/developer/marketing/api/list-members/add-member-to-list/).

```php
<?php

function modify_mailchimp_request( $request, $form, $args ) {
    // The request body is JSON encoded so we must first decode it
    $body = json_decode( $request['body'], true );

    // Set custom merge tag named "NICKNAME" from "nickname" field 
    $body['merge_fields']['NICKNAME'] = af_get_field( 'nickname' );

    // Re-encode the altered body as JSON
    $request['body'] = json_encode( $body, JSON_FORCE_OBJECT );

    return $request;
}
add_filter( 'af/form/mailchimp/request/key=FORM_KEY', 'modify_mailchimp_request', 10, 3 );
```

## Modifiers

- `af/form/mailchimp/request` — Applies to all forms.
- `af/form/mailchimp/request/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/mailchimp/request/id=FORM_ID` — Applies to forms with a specific post ID.
