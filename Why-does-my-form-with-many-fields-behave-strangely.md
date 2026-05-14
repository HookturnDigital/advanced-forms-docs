# Why does my form with many fields behave strangely?

If a form with a lot of fields starts misbehaving on submission — values appearing to vanish, validation firing on fields the user actually filled out, or submissions getting silently truncated — the cause is almost always PHP's `max_input_vars` limit.

`max_input_vars` caps how many input variables PHP will parse out of a single request. The default is `1000`. When a form sends more than that, PHP silently drops the extras before Advanced Forms (or ACF) ever sees them. AF then iterates the inputs it *did* receive, and any field whose value got dropped looks empty — which is what produces the missing-values / unexpected-validation behaviour.

Forms most likely to hit this:

- Repeaters with many rows (each subfield counts as its own input).
- Forms with several conditional groups that all expand at once.
- Long forms that combine multiple field groups.

## Raising `max_input_vars`

Where you set it depends on the hosting setup:

- **Self-managed / VPS:** edit `php.ini` and set `max_input_vars = 5000` (or higher), then restart PHP-FPM.
- **Shared hosting:** most control panels expose a PHP settings UI — cPanel's *Select PHP Version → Options*, Plesk's *PHP Settings*, etc. Some hosts also honour `php_value max_input_vars 5000` in `.htaccess`, but many disable that — try the control panel first.
- **Managed WordPress (Kinsta, WP Engine, etc.):** ask support. They'll usually raise it for you, and on some plans it's already set well above the default.

## Confirming what's actually set

Drop `<?php phpinfo(); ?>` into a temporary page or mu-plugin and search the output for `max_input_vars`. You'll see two columns:

- **Local Value** — what's applied for this request (this is the one that matters).
- **Master Value** — the system default.

If the Local Value is still `1000` after you've changed your `php.ini` or control panel setting, the change hasn't taken effect — PHP-FPM may need a restart, or the setting is being overridden somewhere else in the load order.
