# `af/form/button_attributes`

Filter attributes on the submit button. Use to add classes, set an ID, or add new attributes.

`$attributes` is an array of HTML attributes and their values.

```php
<?php

function filter_submit_button_attributes( $attributes, $form, $args ) {
    $attributes['class'] .= ' button';
    
    return $attributes;
}
add_filter( 'af/form/button_attributes/key=FORM_KEY', 'filter_submit_button_attributes', 10, 3 );
```

## Modifiers

- `af/form/button_attributes` — Applies to all forms.
- `af/form/button_attributes/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/button_attributes/id=FORM_ID` — Applies to forms with a specific post ID.
