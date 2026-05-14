# How to trigger FacetWP's indexer for posts managed by an ACF form

> **Third-party note:** This article describes integration with **FacetWP**, a third-party plugin. The integration approach shown here was correct at the time of writing — third-party plugins may change their APIs, hooks, or behaviours over time, so verify against the current FacetWP documentation if you hit issues.

If you're using FacetWP to render filtered post listings and posts are being created or updated via an Advanced Forms form, you'll often find FacetWP's index doesn't pick up the changes automatically — the filtered results stay stale until the next scheduled re-index. The fix is to nudge FacetWP's indexer manually whenever AF finishes handling a post submission, by hooking in late on `init` and passing the submission's post ID to `FWP()->indexer->index()`:

```php
add_action( 'init', function () {

    if ( ! function_exists( 'AF' ) ) {
        trigger_error( 'AF() function is unavailable — the Advanced Forms plugin may not be active.' );
        return;
    }

    if ( ! function_exists( 'FWP' ) ) {
        trigger_error( 'FWP() function is unavailable — the FacetWP plugin may not be active.' );
        return;
    }

    // Only act when AF's current submission references a real post ID.
    if ( empty( AF()->submission['post'] ) || ! is_int( AF()->submission['post'] ) ) {
        return;
    }

    // Make sure FacetWP's indexer is available before calling it.
    if ( empty( FWP()->indexer ) ) {
        trigger_error( 'FWP()->indexer instance was not available — could not trigger index on post ID.' );
        return;
    }

    // Pass the indexer the post ID so FacetWP re-indexes just this post.
    FWP()->indexer->index( AF()->submission['post'] );

}, 30 );
```

Drop this into a custom plugin, an MU-plugin, or your theme's `functions.php` — anywhere that loads on every request is fine. The late priority (`30`) on `init` gives Advanced Forms time to populate `AF()->submission` before the callback runs.

The `trigger_error()` guards are there so you'll see a useful PHP notice in your error log if either plugin is deactivated or FacetWP's internals shift in a future release — better than failing silently.

If you'd rather scope this to a specific form rather than running for every AF submission, gate it on `AF()->submission['form']['key']`:

```php
if ( AF()->submission['form']['key'] !== 'YOUR_FORM_KEY_HERE' ) {
    return;
}
```
