# What to do when there are JavaScript errors affecting a form

**Symptom:** A form's fields don't render correctly, validation misbehaves, the submit button does nothing, or the browser console shows JavaScript errors when the page loads.

**Most likely cause:** A third-party plugin interfering with Advanced Forms' (or ACF's) scripts. The usual suspects are:

- **Optimisation plugins** that defer, async, combine, or minify scripts (often breaking the load order or executing scripts before their dependencies).
- **Caching plugins** serving stale JS bundles after an update.
- **Other form plugins** that re-implement jQuery global handlers or override `acf` namespaces.
- **Theme JavaScript** that throws an error early enough to halt subsequent script execution.

## How to track down the conflict

The fastest way to find the responsible plugin is the standard WordPress bisection:

1. Make sure you're testing in a normal browser session (not a private/incognito tab that might cache differently).
2. **Deactivate all plugins except Advanced Forms, ACF, and any plugins essential to the page rendering.** Test the form.
3. If the form works: reactivate the other plugins one by one, testing the form between each. The plugin you reactivate immediately before the form breaks is the culprit.
4. If the form *still* breaks: switch to a default theme (e.g. Twenty Twenty-Four). If it works under the default theme, the issue is in your theme's JavaScript.

For optimisation-plugin issues specifically, exclude Advanced Forms and ACF scripts from any defer/async/combine/minify rules:

- `af-forms-script`
- `acf-input`
- `acf-input-*` (any ACF field-type-specific scripts)
- `jquery-core`, `jquery-migrate`

## If you've identified the conflict and need help

Open a support ticket with: the plugin you've identified, the page URL where the form lives, a screenshot of the browser console showing the error, and any relevant config (e.g. the optimisation rules you're using if it's a perf plugin). We'll help work out a permanent fix or workaround.
