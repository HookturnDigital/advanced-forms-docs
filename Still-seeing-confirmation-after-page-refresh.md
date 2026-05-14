# Still seeing the confirmation message after a page refresh

**Symptom:** A visitor submits a form, sees the success/confirmation message, and then on refreshing the page (or returning to it later) they see the confirmation message again — as if the form was just submitted, even though it wasn't.

**Cause:** A cached version of the post-submission page is being served. Advanced Forms renders the confirmation by re-rendering the page in its "just submitted" state; if that response gets stored by a page cache, every subsequent visitor to that URL gets the same cached "you just submitted the form" HTML until the cache is purged.

**Fix:** Exclude the page that contains the form from page caching. Where you configure that depends on what's doing the caching:

- **A caching plugin** (WP Rocket, W3 Total Cache, LiteSpeed Cache, WP Super Cache, etc.) — each has a "never cache these pages / URLs" setting. Add the form's page URL there.
- **Server-level caching** (LiteSpeed, Varnish, NGINX FastCGI cache, host-managed full-page cache like Kinsta/WP Engine/SiteGround) — the plugin settings can't always reach this layer. Ask your host's support to exclude the page, or use whatever cache-exclusion mechanism their control panel exposes.
- **A CDN with HTML caching enabled** (Cloudflare APO, Bunny, etc.) — exclude the URL from edge HTML caching too.

If you're not sure which layer is caching the page, the quickest test is to load the page in a private/incognito window immediately after submitting in another browser — if the confirmation shows up there too, it's being cached somewhere along the chain.

## See also

- [What to do when there are JavaScript errors affecting a form](./what-to-do-when-there-are-javascript-errors-affecting-a-form/) — for the related class of issues where a form misbehaves because of plugin interference rather than caching.
