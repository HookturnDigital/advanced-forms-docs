# Can users access the media library via Advanced Forms?

Yes — if a user is logged in and has permission to access the media library, upload fields will default to using it. Which uploader renders is controlled by the [`uploader` display argument](./display-arguments/) when you render the form.

The two relevant values are:

- `wp` — the WordPress media library UI. Requires a logged-in user with the `upload_files` capability. This is the default when the current user qualifies.
- `basic` — the plain HTML file input. No capabilities required, works for guests, but no media library UI.

If you want a consistent experience for everyone regardless of login state, force `uploader` to `basic`. If you want logged-in users with the right caps to get the full media library while guests fall back gracefully, leave it on the default.

Two related capability gotchas come up often enough that they have their own pages:

- [Gallery field not working for some users](./gallery-field-not-working-for-some-users/) — gallery fields need `upload_files`, and they can't work for non-logged-in users at all.
- ["Sorry, you are not allowed to attach files to this post"](./sorry-you-are-not-allowed-to-attach-files-to-this-post-when-adding-media-to-a-post/) — surfaces when the user can authenticate to the media library but lacks the `edit_*` capability for the post the upload is being attached to.

## How can I restrict the available media when using the uploader?

There's no built-in mechanism in Advanced Forms for this — restrictions on which media a user can see in the library are best handled at the WordPress level rather than in the form. WordPress treats the media library as a single shared pool by default, so any user with `upload_files` sees everything.

There are plugin- and code-based approaches that scope the library per-user (showing each user only their own uploads, or filtering by role). [This GreenGeeks article](https://www.greengeeks.com/tutorials/protect-media-library-wordpress-users-uploads/) walks through both options and is a reasonable starting point.

Once the WordPress-level restriction is in place, the Advanced Forms uploader inherits it automatically — no extra configuration needed on the form side.
