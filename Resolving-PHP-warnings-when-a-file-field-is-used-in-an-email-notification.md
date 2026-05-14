# Resolving PHP warnings when a file field is used in an email notification

**Symptom:** PHP warnings appear in the error log (or on-screen, if `WP_DEBUG_DISPLAY` is on) when a form submission triggers an email notification that references an ACF file field — typically `Trying to access array offset on value of type bool/int/string`, or `Undefined index: url` / `Undefined index: filename`.

**Cause:** Advanced Forms' email-notification handling expects the file field to return an array (the full attachment object — `id`, `url`, `filename`, etc.). When the field's **Return Format** is set to **File URL** or **File ID**, the merge-tag logic tries to read array keys off a string or integer, and PHP warns.

**Fix:** Update Advanced Forms to the latest version — the file-field handling has been hardened to cope with all three return formats. If you can't update right away, change the file field's **Return Format** to **File Array** under the field group's edit screen and re-save the field group.

If you want to keep the field's return format as-is for other reasons (e.g. other code on the site already consumes the URL or ID directly), the warnings will go away once you're on a recent version of Advanced Forms.

## See also

- [How to add attachments to a notification email](./how-to-add-attachments-to-a-notification-email/) — covers attaching the uploaded file itself to the outgoing email.
- [Customising email notifications](./customizing-email-notifications/) — the merge-tag and template basics for notification emails.
