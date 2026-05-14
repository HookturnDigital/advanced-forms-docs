# How to enable all pages on page load for a multi-page form

By default, each page in a multi-page form only becomes navigable once the user has clicked **Next** to reach it — this is so the form can validate each page in turn. There's no built-in option to make every page navigable from page load, but a small JavaScript snippet does the trick:

```js
jQuery(function ($) {

    acf.addAction('af/form/setup', function (form) {

        // Total number of pages on this form.
        const n_pages = form.pages.length;

        // Bump max_page to the last page (zero-indexed) so all pages are
        // marked as already reached.
        form.max_page = n_pages - 1;

        // Refresh the form UI — page links will now render as clickable.
        af.pages.refresh(form);

    });
});
```

Drop the snippet into your site's footer or a custom JS file enqueued on pages that render the form.

Note: skipping the per-page validation gate means you can land on a later page with invalid earlier-page data. If that's a concern, look at validating on submission instead of relying on the gated page progression.
