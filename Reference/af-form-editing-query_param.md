# `af/form/editing/query_param`

Filter for the URL query parameter used to identify the post a form should edit, when the form's `post` argument is set to `param`. Defaults to `post` (so `?post=123` selects post 123 for editing). Change it if `post` clashes with another query parameter on your site.

```php
add_filter( 'af/form/editing/query_param', function ( $query_param, $form, $args ) {
    return 'edit_post_id';
}, 10, 3 );
```

With the filter above, the form would look for the post ID in `?edit_post_id=123` instead.

The filter receives the current `$query_param` (defaults to `'post'`), the `$form` array, and the `$args` array. Return the desired parameter name as a string.

## Modifiers

- `af/form/editing/query_param` — Applies to all forms with `post=param`.
- `af/form/editing/query_param/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/editing/query_param/id=FORM_ID` — Applies to forms with a specific post ID.

## See also

- [Use a custom URL parameter to edit a specific post ID](../custom-url-parameter-edit-post/)
- [Creating and editing posts](../creating-and-editing-posts/)
