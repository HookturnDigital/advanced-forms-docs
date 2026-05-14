# `af/form/editing/post_updated`

:::caution[Pro feature]
This hook is only available in Advanced Forms Pro.
:::

Triggered after a post has been edited. Not triggered when a post is created.

```php
<?php

function form_post_updated( $post, $form, $args ) {
    // Do something with the edited post.
    // $post is a WP_Post object.
}
add_action( 'af/form/editing/post_updated/key=FORM_KEY', 'form_post_updated', 10, 3 );
```

## Modifiers

- `af/form/editing/post_updated` — Applies to all forms.
- `af/form/editing/post_updated/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/editing/post_updated/id=FORM_ID` — Applies to forms with a specific post ID.
