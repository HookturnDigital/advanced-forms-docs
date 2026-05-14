# 'Non-human user detected' message when submitting a form

**Symptom:** A legitimate front-end form submission is rejected with a "Non-human user detected" error message. The user filled the form in by hand, but Advanced Forms refuses to accept it.

**Cause:** That message comes from Advanced Forms' built-in honeypot — a hidden input that real users never see and never fill in. If anything submits a value in that field, AF assumes the request came from a bot and blocks it. The two common reasons a real user trips it:

- **Browser auto-fill** ignoring the field's `autocomplete="off"` attribute and dropping a value into the hidden input anyway (most often the user's name, email, or address).
- **CSS from a theme or another plugin** unhiding the honeypot field, so the user can see it and types something into it.

## Fix

Start by checking which of the two is happening. View the page source (or inspect the form in DevTools) and find the honeypot input — it sits inside the form markup with `autocomplete="off"` and is hidden via inline styles. If it's visible on the page, the cause is a CSS conflict; if it's hidden but still receiving a value on submit, the cause is auto-fill.

**If the field is visible**, track down the CSS rule unhiding it (usually a theme or form-plugin rule targeting hidden inputs or the honeypot's wrapper) and scope it more tightly so it doesn't touch AF's honeypot.

**If the field is invisible but still being auto-filled**, and you can't stop the offending browser extension or password manager from ignoring `autocomplete="off"`, the cleanest fix is to disable the honeypot for that specific form via the `af/form/args` filter:

```php
add_filter( 'af/form/args/key=YOUR_FORM_KEY_HERE', function ( $args ) {
    $args['honeypot'] = false;
    return $args;
} );
```

Disabling the honeypot removes AF's lightweight bot protection on that form, so if the form is publicly reachable consider pairing it with another anti-spam measure (a CAPTCHA field, or a server-side check inside an `af/form/submission` callback).

## See also

- [What to do if a browser auto-fills the honeypot field, causing a form to be invalid](./what-to-do-if-a-browser-auto-fills-the-honeypot-field-causing-a-form-to-be-invalid/) — the auto-fill scenario in more detail.
