# Why is my success message not showing after submission?

**Symptom:** A visitor submits a form and never sees the success/confirmation message. The page reloads to its pre-submission state, shows the default page content, or otherwise skips the post-submission block entirely.

The usual culprit is page caching. Advanced Forms renders the confirmation by re-rendering the same page in its "just submitted" state — so if a cache layer is serving a stored copy of the page, the visitor never sees the post-submission response, only the pre-cached version of the form. Less common causes are a theme template overriding the form's container before the success block can render, or a `redirect` display argument pointing to a URL that doesn't include the form (and therefore has no success block to show).

You've got two practical fixes — pick whichever fits your setup better.

## Disable caching on the form's page

Exclude the page that contains the form from page caching. Where you configure that depends on what's doing the caching:

- **A caching plugin** (WP Rocket, W3 Total Cache, LiteSpeed Cache, WP Super Cache, etc.) — each has a "never cache these pages / URLs" setting. Add the form's page URL there.
- **Server-level caching** (LiteSpeed, Varnish, NGINX FastCGI cache, host-managed full-page cache like Kinsta/WP Engine/SiteGround) — the plugin settings can't always reach this layer. Ask your host's support to exclude the page, or use whatever cache-exclusion mechanism their control panel exposes.
- **A CDN with HTML caching enabled** (Cloudflare APO, Bunny, etc.) — exclude the URL from edge HTML caching too.

If you're not sure which layer is caching the page, load it in a private/incognito window right after submitting in another browser. If the form still looks unsubmitted there too, the page isn't being served from cache and the cause is elsewhere — most likely a theme template wrapping or replacing the form's container before the success block renders.

## Or: redirect to a dedicated thank-you page

The cleaner option in a lot of cases is to sidestep the issue entirely — send the visitor to a separate "thanks for your submission" page after they submit. The form's page can stay cached, and the thank-you page is just a plain static page with no form on it, so caching doesn't get in the way.

Create the thank-you page in WordPress, then pass its URL to the [`redirect` display argument](./customizing-redirect-behaviour/) when you render the form. Advanced Forms will send the visitor there immediately after a successful submission.

This also gives you a cleaner UX moment — a dedicated page where you can thank the visitor properly, set expectations about what happens next, link to related resources, or fire off conversion tracking — rather than relying on an inline confirmation block to do all of that work.

## See also

- [Still seeing the confirmation message after a page refresh](./still-seeing-confirmation-after-page-refresh/) — the *opposite* symptom (the confirmation persists when it shouldn't), also caused by page caching.
- [Customising redirect behaviour](./customizing-redirect-behaviour/) — full details on the `redirect` display argument.
