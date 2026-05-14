# Emails are sending with the incorrect 'from' email address

**Symptom:** A form's email notification arrives with a different `From` address (or `From` name) than the one configured in the form's email settings.

**Cause:** Almost always one of two things:

1. **Malformed value in the From field.** The `From` setting needs to be either a bare email address or in the format `Name <email@example.com>`. Anything else gets dropped or rewritten.
2. **A third-party plugin rewriting `wp_mail()` headers.** SMTP plugins (WP Mail SMTP, Post SMTP, Easy WP SMTP, FluentSMTP, etc.) commonly force a "verified sender" From address regardless of what the calling code passed in — this is intentional, because mail providers reject mismatched senders for deliverability reasons. Some security and notification plugins do the same.

**Fix:**

1. Check the `From` value in the form's email settings is one of:

   - `email@example.com`
   - `Sender Name <email@example.com>`

2. If the formatting is correct, deactivate any plugin that touches outgoing mail (SMTP plugins especially) and send a test notification. If the From address now respects the form setting, that plugin is the one overriding it.

3. Once you've identified the plugin, you have two options:

   - **Configure the SMTP plugin to allow per-message From addresses.** Most have a setting along the lines of "Force From Email" / "Force From Name" — turn those off so Advanced Forms' value is preserved. Note that your sending domain still needs to be verified with your mail provider for the message to actually deliver.
   - **Set the From address via the `af/form/email/headers` filter** if you need it controlled in code rather than the UI. See [Adding custom email headers to notification emails](./adding-custom-email-headers-to-notification-emails/).

If the SMTP plugin is forcing a From address deliberately (because only one verified sender is configured with the mail provider), that's working as intended — change the From at the provider level, or verify the address you want to use as a sender.

## See also

- [Customising email notifications](./customizing-email-notifications/)
- [Adding custom email headers to notification emails](./adding-custom-email-headers-to-notification-emails/)
- [Email notifications contain relative file paths for downloads](./email-notifications-contain-relative-file-paths-for-downloads/)
