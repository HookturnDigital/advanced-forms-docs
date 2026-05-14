# How to fix encoding issues with special characters in Apple Mail subject lines

**Symptom:** A notification email's subject line appears garbled when the recipient opens it in Apple Mail — typically rendered as a string of mojibake or quoted-printable gibberish instead of the intended text — while the same email looks fine in other clients.

**Cause:** We've had a report that certain characters in the subject line can trip an encoding issue specific to Apple Mail. In the reported case, the culprit was commas in the subject — removing them resolved the display and the subject rendered correctly again. We haven't catalogued every character that triggers this, so treat it as "Apple Mail is fussy about subject-line encoding under some conditions" rather than a hard rule.

**Fix:** Avoid the offending characters in the subject, or strip/replace them programmatically before the email is sent using the [`af/form/email/subject`](../af-form-email-subject/) filter.

```php
add_filter( 'af/form/email/subject', function ( $subject, $email, $form, $args ) {

    // Only customise the subject for the form we care about.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $subject;
    }

    // Strip commas — known to cause encoding issues in Apple Mail.
    return str_replace( ',', '', $subject );

}, 10, 4 );
```

Adjust the `str_replace()` call to target whichever character is causing the issue in your case.

## See also

- [Customising email notifications](./customizing-email-notifications/) — the broader picture of how email notifications are built and where the subject filter fits in.
