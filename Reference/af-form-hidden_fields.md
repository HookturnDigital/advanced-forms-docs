# `af/form/hidden_fields`

Use this action to add hidden inputs which can contain data you want to pass along your form, for example the current post title as shown in the example below.

The hidden fields will be available in all submission hooks, such as `af/form/submission`, and can be accessed through the `$_POST` object. Please keep in mind that hidden inputs are not ACF fields and won't be saved automatically to entries or posts.

```php
<?php

function hidden_field( $form, $args ) {
    // The title can later be retrieved using $_POST['post_title'].
    $title = get_the_title();
    echo sprintf( '<input type="hidden" name="post_title" value="%s">', $title );
}
add_action( 'af/form/hidden_fields/key=FORM_KEY', 'hidden_field', 10, 2 );
```

## Modifiers

- `af/form/hidden_fields` — Applies to all forms.
- `af/form/hidden_fields/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/hidden_fields/id=FORM_ID` — Applies to forms with a specific post ID.
