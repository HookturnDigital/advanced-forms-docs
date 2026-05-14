# How to trigger a calculated field update

You can force a calculated field to recalculate from JavaScript — useful when the calculation depends on something that doesn't fire ACF's default change events (a custom UI element, an async data source, etc.).

The full pattern is in the ACF docs: [Extending calculated fields with JavaScript](../working-with-calculated-fields/#extending-calculated-fields-with-javascript).

## Watch the request volume

Every calculated-field update round-trips to the server via AJAX, so the update isn't free. Things to keep in mind:

- **Don't bind to high-frequency events** (`keyup`, `mousemove`, `scroll`) without debouncing — you'll fire many overlapping requests.
- **Overlapping requests can return out-of-order**, which means a stale calculation can overwrite a fresh one. Debounce so only one request is in flight at a time.
- **Expect a visible delay** between the trigger and the updated preview — the calculation runs on the server, so it's not instant even on a fast connection.

If the calculation is fully expressible in JavaScript (no database lookups, no PHP-only data), consider doing it client-side instead and skipping the AJAX round-trip entirely.
