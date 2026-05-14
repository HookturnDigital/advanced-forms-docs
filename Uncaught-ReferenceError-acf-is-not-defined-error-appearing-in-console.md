# Uncaught ReferenceError: acf is not defined

**Symptom:** The browser console shows `Uncaught ReferenceError: acf is not defined`, and the form on the page misbehaves — fields don't initialise, validation doesn't fire, or interactive controls (date picker, wysiwyg, media buttons) sit dead.

**Cause:** The `acf` JavaScript global hasn't been defined by the time code that depends on it runs. Two common reasons:

1. **An asset-optimisation plugin is interfering with how ACF's scripts load.** Plugins that combine, defer, async, or minify scripts often struggle with dynamically enqueued assets like the ones Advanced Forms (and ACF) rely on. The result is `acf-input` either failing to load, loading in the wrong order, or executing after code that already expects `acf` to exist.
2. **A jQuery UI dependency has been deregistered.** Advanced Forms depends on ACF, which depends on `jquery-ui-sortable` and `jquery-ui-resizable`, which in turn depend on `jquery-ui-core`. Some "speed up WordPress" plugins or theme tweaks deregister these lower-level scripts. When that happens, WordPress drops the higher-level dependencies too — and `acf-input` never gets printed onto the page.

**Fix:** Work through these in order.

1. **Temporarily deactivate asset-optimisation plugins** (anything doing combine / defer / async / minify on scripts). Reload the page. If the error goes away, the optimisation plugin is the culprit — re-enable it and exclude the following handles from any combine / defer / async rules:

   - `acf-input`
   - `af-forms-script`
   - `jquery-core`, `jquery-migrate`
   - any `acf-input-*` field-type scripts (e.g. `acf-input-date-picker`)

2. **Check whether jQuery UI has been deregistered.** Look in your theme or active plugins for calls like `wp_deregister_script( 'jquery-ui-core' )` (or `jquery-ui-sortable`, `jquery-ui-resizable`). Remove them. If you need to keep them deregistered for performance reasons, you'll need to re-register the specific handles ACF depends on so the dependency chain stays intact.

3. **Confirm the page actually has an ACF context.** If the form is being rendered out-of-band (e.g. injected via AJAX into a page that doesn't normally render an Advanced Form), ACF's assets won't be enqueued at all. See the AJAX-rendering doc linked below for the `af_enqueue()` fix.

If none of the above resolves it, the page-load error is most likely a different JavaScript error firing earlier and halting execution before `acf` gets a chance to define itself. Open the console, look for the *first* red error on the page, and start there.

## See also

- [Scripts & stylesheets aren't loaded when a form is rendered via AJAX](./scripts-and-stylesheets-arent-loaded-when-a-form-is-rendered-via-ajax/) — when the issue is that ACF's assets simply weren't enqueued for the request.
- [What to do when there are JavaScript errors affecting a form](./what-to-do-when-there-are-javascript-errors-affecting-a-form/) — bisection workflow for tracking down which plugin or theme is the source.
