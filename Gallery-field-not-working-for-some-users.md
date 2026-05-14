# Gallery field not working for some users

**Symptom:** A form containing a gallery field works for some users but not others. For the affected users, clicking the **Add to gallery** button does nothing — the media uploader doesn't open, and the browser console logs an error about a missing media-uploader script or undefined property (typically a `wp.media` call failing because the media-uploader assets were never loaded for that user).

**Cause:** This isn't a bug in Advanced Forms — it's a WordPress capability check. WordPress only enqueues the media-uploader scripts and resources for users who have the `upload_files` capability. Users without that cap get a form page with no media uploader behind the gallery field, so the **Add to gallery** click has nothing to bind to.

In practice, this means the gallery field can only be used in a front-end form by **logged-in users who have the `upload_files` capability**. By default that's Administrators, Editors, Authors, and Contributors — Subscribers and custom roles don't have it unless you grant it explicitly.

**Fix:** Grant the `upload_files` capability to the roles that need to use the gallery field. You may consider using a plugin like [User Role Editor](https://wordpress.org/plugins/user-role-editor/) for a UI-driven approach, or do it in code on plugin/theme activation:

```php
add_action( 'init', function () {
    $role = get_role( 'subscriber' );

    if ( $role && ! $role->has_cap( 'upload_files' ) ) {
        $role->add_cap( 'upload_files' );
    }
} );
```

Capabilities are stored in the database, so you only need this to run once per environment — wrap it in an activation hook, or run it manually and remove the code afterwards.

## If you need to support non-logged-in users

The gallery field genuinely can't work for non-logged-in users — the media uploader is a logged-in-only WordPress feature, and there's no capability you can grant to a visitor who isn't authenticated.

If you need to collect multiple files from non-logged-in users, use a **repeater field containing a regular image or file field** instead. Those don't depend on the media uploader and work fine for guests.

## See also

- [Sorry, you are not allowed to attach files to this post](./sorry-you-are-not-allowed-to-attach-files-to-this-post-when-adding-media-to-a-post/) — a related capability issue that surfaces when image/file fields try to attach uploads to a post the user can't edit.
