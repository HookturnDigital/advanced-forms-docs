# `af/form/editing/post_created`

:::caution[Pro feature]
This hook is only available in Advanced Forms Pro.
:::

Triggered after a post has been created. Not triggered when a post is edited.

```php
<?php

function form_post_created( $post, $form, $args ) {
    // Do something with the created post.
    // $post is a WP_Post object.
}
add_action( 'af/form/editing/post_created/key=FORM_KEY', 'form_post_created', 10, 3 );
```

## Modifiers

- `af/form/editing/post_created` — Applies to all forms.
- `af/form/editing/post_created/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/editing/post_created/id=FORM_ID` — Applies to forms with a specific post ID.
