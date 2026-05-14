# `af/form/previous_button_atts`

Filter attributes on the previous button in multi-page forms. Use to add classes, set an ID, or add new attributes.

`$attributes` is an array of HTML attributes and their values.

```php
<?php

function filter_previous_button_attributes( $attributes, $field ) {
    $attributes['class'] .= ' button';
    
    return $attributes;
}
add_filter( 'af/form/previous_button_atts', 'filter_previous_button_attributes', 10, 2 );
```
