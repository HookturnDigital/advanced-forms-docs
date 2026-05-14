# Fields are empty when using a form to edit a post

**Symptom:** A form intended to edit an existing post renders with empty fields, and submitting it creates a new post rather than updating the intended one.

**Cause:** The form has no `post` argument, so Advanced Forms defaults to its "create new post" behaviour. Without a target post, there's nothing to pre-populate the fields with — and on submit, a new post is what you get.

**Fix:** Tell the form which post to edit by passing a `post` argument on the shortcode or in the array passed to `advanced_form()`.

To edit the current post (handy inside The Loop or on a singular template):

```text
[advanced_form form="YOUR_FORM_KEY_HERE" post="current"]
```

To edit a specific post by ID:

```text
[advanced_form form="YOUR_FORM_KEY_HERE" post="123"]
```

Or, when rendering the form directly in PHP:

```php
advanced_form( 'YOUR_FORM_KEY_HERE', [
    'post' => 123,
] );
```

Once the `post` argument is set, the form will load that post's existing field values and updates will save back to the same post.
