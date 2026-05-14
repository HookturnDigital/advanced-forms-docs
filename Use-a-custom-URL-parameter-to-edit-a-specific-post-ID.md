# Use a custom URL parameter to edit a specific post ID

To let a single page edit any post by ID, set the form's `post` arg to `"param"` — Advanced Forms will then read the target post ID from a query parameter on the current URL.

## Configuring the form

Using the shortcode:

```
[advanced_form form="YOUR_FORM_KEY_HERE" post="param"]
```

Or with the function:

```php
advanced_form( 'YOUR_FORM_KEY_HERE', [ 'post' => 'param' ] );
```

With that in place, the form looks for a `post` query parameter on the request URL. For example, the URL `https://example.com/edit-post?post=123` will load the form configured to edit post `123`.

## Customising the query parameter

If `post` clashes with something else on your site, you can change the parameter name with the [`af/form/editing/query_param`](../af-form-editing-query_param/) filter:

```php
add_filter( 'af/form/editing/query_param', function ( $query_param, $form, $args ) {
    return 'some_custom_param';
}, 10, 3 );
```

With that filter in place, the form looks for `some_custom_param` instead — e.g. `https://example.com/edit-post?some_custom_param=123`.

## See also

- [How to redirect the form submission after post creation](./redirect-after-post-creation/) — pair this with a redirect that includes the new post's ID for a create-then-edit flow.
