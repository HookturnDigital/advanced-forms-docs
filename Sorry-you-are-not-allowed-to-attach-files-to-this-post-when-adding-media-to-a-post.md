# "Sorry, you are not allowed to attach files to this post" when adding media to a post

**Symptom:** A user submitting a form that includes a file or image upload field sees the error:

> Sorry, you are not allowed to attach files to this post

The upload is rejected before the file ever reaches the media library.

**Cause:** The user filling in the form doesn't have the WordPress capability required to edit the post the upload is being attached to. WordPress's media-upload AJAX handler (in `wp-admin/includes/ajax-actions.php`) does a capability check against the target post before accepting the attachment — if it fails, you get this error.

The capability it checks is either `edit_post` or a post-type-specific variant of it, depending on how the target post type was registered. WordPress derives the variant from the post type's `capability_type` argument:

- If `capability_type` is left at its default (`post`), the required capability is `edit_post`.
- If `capability_type` is set to `page`, the required capability is `edit_page`.
- If `capability_type` is set to something custom (e.g. `'capability_type' => 'product'`), the required capability is `edit_product` — and you'll also need to have mapped meta capabilities properly via `map_meta_cap` for it to resolve at runtime.

**Fix:** Grant the appropriate capability to the role(s) of the users filling in the form.

The cleanest way to manage capabilities without writing code is to use a plugin like [User Role Editor](https://wordpress.org/plugins/user-role-editor/) — find the role that needs the capability (often `subscriber` or a custom front-end role), tick the relevant `edit_*` capability, save.

If you'd rather handle it in code, add the capability to the role on plugin/theme activation:

```php
add_action( 'admin_init', function () {
    $role = get_role( 'subscriber' );
    if ( $role && ! $role->has_cap( 'edit_posts' ) ) {
        $role->add_cap( 'edit_posts' );
    }
} );
```

Substitute the role name and capability for whatever your form's target post type actually needs. Be deliberate here — granting `edit_posts` to `subscriber` lets every subscriber edit posts site-wide, which is usually not what you want. For front-end submissions, a dedicated custom role scoped narrowly to the post type you care about is the safer pattern.

## See also

- [Gallery field not working for some users](./gallery-field-not-working-for-some-users/) — same capability story, different upload field.
