# 'Add new tag' button missing on taxonomy field

**Symptom:** A taxonomy field with **Create Terms** enabled is rendered on a front-end form, but the "Add new tag" button/icon doesn't appear for some users — even though the field itself is showing fine.

**Cause:** ACF only exposes the inline term-creation control to users who hold the **`manage_terms`** capability for the relevant taxonomy. Logged-in users without that cap (and logged-out visitors) see the field but not the add button — by design.

The exact capability name depends on the taxonomy. Each taxonomy maps `manage_terms` to its own underlying cap, which you can inspect via `get_taxonomy()`:

```php
get_taxonomy( 'category' )->cap->manage_terms;
// 'manage_categories'

get_taxonomy( 'post_tag' )->cap->manage_terms;
// 'manage_post_tags'
```

Custom taxonomies registered with `register_taxonomy()` either inherit defaults or define their own caps via the `capabilities` argument — check what your taxonomy is actually mapped to before granting anything.

**Fix:** Grant the relevant `manage_terms` capability to the user role that needs to add terms from the front-end form. The [User Role Editor plugin](https://wordpress.org/plugins/user-role-editor/) is the easiest way to manage this through the admin UI. If you'd rather do it in code:

```php
add_action( 'init', function () {
    $role = get_role( 'subscriber' );

    if ( $role && ! $role->has_cap( 'manage_categories' ) ) {
        $role->add_cap( 'manage_categories' );
    }
} );
```

Be deliberate about which role you grant this to — `manage_terms` lets users create, edit, and delete terms in that taxonomy site-wide, not just via the front-end form.
