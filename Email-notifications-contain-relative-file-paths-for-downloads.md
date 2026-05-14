# Email notifications contain relative file paths for downloads

**Symptom:** A form submission triggers an email notification that includes a file attachment link, but the link is rendered as a relative path (`/wp-content/uploads/2024/01/example.pdf`) instead of an absolute URL (`https://www.example.com/wp-content/uploads/2024/01/example.pdf`). Recipients can't click the link — email clients have no base URL to resolve it against.

**Cause:** Advanced Forms uses WordPress' underlying APIs (`wp_get_attachment_url()` and friends) to build the URL, which normally returns a fully-qualified absolute URL. If you're seeing relative paths, something on the site is filtering attachment URLs into a relative format before Advanced Forms gets a chance to render them into the email body. This is almost always another plugin or a snippet of theme code hooking into `wp_get_attachment_url`, `upload_dir`, or similar filters and stripping the host.

**Fix:** Disable other plugins one at a time until the URLs come back as absolute. The plugin you deactivate immediately before the fix takes effect is the one rewriting URLs.

## Plugins known to cause this

- **Sage Soil** — converts URLs to relative paths as part of its asset rewriting.

If the offending plugin is one you genuinely need, check its settings for a way to scope the URL rewriting to the front-end only (most have a "skip in admin / cron / REST" option). If it doesn't expose one, the plugin's `wp_get_attachment_url` filter is what's stripping the host — either disable it for the relevant request context or replace it with a more targeted version.

## See also

- [How to add attachments to a notification email](./how-to-add-attachments-to-a-notification-email/)
- [Customising email notifications](./customizing-email-notifications/)
- [Resolving PHP warnings when a file field is used in an email notification](./resolving-php-warnings-when-a-file-field-is-used-in-an-email-notification/)
