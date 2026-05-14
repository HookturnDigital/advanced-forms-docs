# How to assign the form markup to a variable

By default `advanced_form()` echoes the form's markup at the call site. To capture the markup into a variable instead, set the `echo` arg to `false` — the function then returns the markup as a string.

```php
$form_html = advanced_form( 'YOUR_FORM_KEY_HERE', [ 'echo' => false ] );
```

Useful when you need to inject the form into a wrapper layout, run it through a shortcode buffer, or render it inside a template engine (Twig, Blade, etc.) that expects a string rather than direct output.

The `[advanced_form ...]` shortcode always echoes — there's no shortcode equivalent of the `echo` arg, so reach for the function when you need the markup as a value.
