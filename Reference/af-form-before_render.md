# `af/form/before_render`

Make changes to a form before it's rendered. Parameter `$form` is a form array.

Can be used for example to modify the form title, description, or success message.

```php
<?php

function filter_form( $form, $args ) {
    $form['display']['description'] = 'New form description';
    
    return $form;
}
add_filter( 'af/form/before_render/key=FORM_KEY', 'filter_form', 10, 2 );
```

## Modifiers

- `af/form/before_render` — Applies to all forms.
- `af/form/before_render/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/before_render/id=FORM_ID` — Applies to forms with a specific post ID.
