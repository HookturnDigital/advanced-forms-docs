# What to do if a browser auto-fills the honeypot field causing a form to be invalid

**Symptom:** A form rejects every submission as bot traffic — typically with a "non-human user detected" message — even when a real person is filling it out. Often only affects some browsers (Safari / iOS Safari are the usual culprits), and tends to coincide with the browser's autofill / password-manager UI showing up.

**Cause:** Advanced Forms ships with a hidden honeypot field that's expected to stay empty — if it has a value at submission time, AF treats the request as a bot and rejects it. Some browsers ignore `autocomplete="off"` on hidden inputs and helpfully autofill the honeypot with the user's name, email, or address from saved profile data. The form is then "invalid" through no fault of the user.

**Fix:** Override the honeypot config so the field name / attributes don't look like something the browser wants to autofill — or disable the honeypot entirely on that form and rely on other bot protection. Both are configured via the `af/form/args` filter (the same filter documented under [honeypot arguments in the AF guides](../display-arguments/#honeypot)).

Rename the honeypot field to something the browser won't recognise as a known profile field:

```php
add_filter( 'af/form/args', function ( $args, $form ) {

    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $args;
    }

    $args['honeypot'] = [
        'field_name' => 'af_hp_token', // anything that isn't name / email / address / phone
    ];

    return $args;

}, 10, 2 );
```

Or disable the honeypot for a specific form and lean on another bot-protection layer (reCAPTCHA, Cloudflare Turnstile, the [non-human-user check](./non-human-user-detected-message-when-submitting-a-form/), etc.):

```php
add_filter( 'af/form/args', function ( $args, $form ) {

    if ( $form['key'] === 'YOUR_FORM_KEY_HERE' ) {
        $args['honeypot'] = false;
    }

    return $args;

}, 10, 2 );
```

Renaming is the first thing to try — it keeps the honeypot working for the 99% of bots that scrape the rendered HTML and submit whatever fields they find. Only fully disable when the rename doesn't shake off the autofill, and pair it with another bot-detection mechanism so the form isn't unprotected.

## See also

- [Non-human user detected message when submitting a form](./non-human-user-detected-message-when-submitting-a-form/) — the message users see when the honeypot (or other bot checks) trip.
