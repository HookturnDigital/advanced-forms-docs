# `af/form/editing/post_data`

:::caution[Pro feature]
This hook is only available in Advanced Forms Pro.
:::

Override the post data used when a post is created or updated. Post data is the `$postarr` argument passed to [`wp_insert_post`](https://developer.wordpress.org/reference/functions/wp_insert_post/).

```php
<?php

function modify_post_data( $post_data, $form, $args ) {
    // Override post title 
    $post_data['post_title'] = 'New post title!';

    return $post_data;
}
add_filter( 'af/form/editing/post_data/key=FORM_KEY', 'modify_post_data', 10, 3 );
```

## Modifiers

- `af/form/editing/post_data` — Applies to all forms.
- `af/form/editing/post_data/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/editing/post_data/id=FORM_ID` — Applies to forms with a specific post ID.
