# How to add an opt-in checkbox for MailChimp

To make the MailChimp integration opt-in on a per-submission basis — only sending to MailChimp when the user ticks an opt-in checkbox — disable the integration on submissions where the checkbox isn't checked.

Add a true/false field to your form (e.g. `add_to_mailchimp`) and override the form's MailChimp configuration in `af/form/submission` based on the value:

```php
add_filter( 'af/form/submission', function ( $form, $fields, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $form;
    }

    // If the opt-in field isn't ticked, disable MailChimp for this submission.
    if ( ! af_get_field( 'add_to_mailchimp' ) ) {
        $form['mailchimp'] = false;
    }

    return $form;

}, 5, 3 ); // Priority 5 — must run before Advanced Forms' MailChimp handler.
```

Priority `5` is important — Advanced Forms' MailChimp handler reads the configured `mailchimp` value during the submission, so the override needs to land before that runs.
