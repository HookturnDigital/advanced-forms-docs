# Why can't I select a post type when editing posts?

**Symptom:** When configuring a form to create or edit posts, some of the site's post types are missing from the **Post type** dropdown — even though they're registered and you can see them elsewhere in WP admin.

**Cause:** A post type only appears in that dropdown if its `show_ui` argument is `true`. If `show_ui` is explicitly `false` (or defaults to `false` because `public` is `false`), Advanced Forms treats the post type as not editable through admin UI and hides it from the list.

**Fix:** Set `show_ui` to `true` on the post type's registration:

```php
add_action( 'init', function () {

    register_post_type( 'my_post_type', [
        'public'  => false,
        'show_ui' => true, // makes the post type appear in the AF Post type dropdown
        // ...
    ] );

} );
```

A couple of things to note:

- If `show_ui` isn't defined at all, it falls back to the value of `public`. So a post type registered with `'public' => false` and no `show_ui` will *also* be hidden from the dropdown.
- `show_ui` only controls whether WP exposes the post type to admin UIs (including AF's form configuration). It doesn't change anything about how the post type behaves on the front end.
