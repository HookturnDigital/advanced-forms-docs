# Can I export form entries?

There's no built-in export mechanism in Advanced Forms, but entries are stored as a standard custom post type (`af_entry`) with their field values saved as ACF data — so any general WordPress export tool that understands posts and post meta will work.

[WP All Export](https://wordpress.org/plugins/wp-all-export/) is a solid option. Point it at the `af_entry` post type, then map the ACF fields you want included in the export.

## See also

- [How to display form entries](./how-to-display-form-entries/) — the same data model, queried on the front end with `WP_Query` / `get_posts()` and ACF's template functions.
