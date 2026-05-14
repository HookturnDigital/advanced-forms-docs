# Fields aren't displaying correctly in the form

**Symptom:** A field renders, but not as the type you configured — e.g. a time picker showing up as a date picker, a wysiwyg rendering as a plain textarea, a select rendering without its Select2 styling, or fields appearing visually broken / unstyled on the front-end.

**Cause:** Advanced Forms renders ACF fields on the front-end and relies on ACF's own scripts and styles to make each field type behave correctly. When another plugin or your theme interferes with that — by dequeueing ACF assets, overriding ACF's JavaScript namespaces, or re-registering field types — the field still renders but the wiring that turns it into a time picker (or wysiwyg, or relationship field, etc.) is missing.

**Fix:** Bisect to find the conflict.

In a safe environment (staging, or a local copy — *not* production):

1. Deactivate all plugins except Advanced Forms and ACF. Reload the form. If the field renders correctly now, a plugin is the culprit.
2. Reactivate plugins one at a time, testing the form between each. The plugin you reactivate immediately before the field breaks again is the offender.
3. If the field is still broken with everything but AF and ACF off, switch to a default theme (e.g. Twenty Twenty-Four). If it works under the default theme, the issue is in your theme.

Once you've identified the conflict, the fix depends on what's interfering — exclude AF/ACF scripts from an optimisation plugin's defer/combine/minify rules, disable a duplicate field-type registration, or work around the theme override. If you'd like a hand, open a support ticket with the plugin or theme you've identified and we'll work out a fix together. Letting us know also helps us spot recurring conflicts and document them.

## See also

- [What to do when there are JavaScript errors affecting a form](./what-to-do-when-there-are-javascript-errors-affecting-a-form/) — the broader version of this guide, covering JS console errors and optimisation-plugin conflicts.
- [Scripts and stylesheets aren't loaded when a form is rendered via AJAX](./scripts-and-stylesheets-arent-loaded-when-a-form-is-rendered-via-ajax/) — if the form is being injected dynamically, the cause is different.
