# How to show form entry data on a 'thank you' page after form submission

When a form redirects to a custom page after submission, you can read the submitted values on that page using `af_get_field()`. Advanced Forms holds the submission in memory for one page load — long enough for the redirect target to render personalised content.

The pattern below registers a shortcode that renders a summary of the submission. You can drop the shortcode into a 'thank you' page directly, or copy the rendering logic into a custom template if you'd rather avoid shortcodes.

```php
/**
 * [afp_entry_summary] — renders a summary of the most recent form submission.
 *
 * Drop into a page that the form redirects to. If there's no live submission
 * (page loaded directly), it falls back to a friendly message.
 */
add_shortcode( 'afp_entry_summary', function () {

    if ( ! af_has_submission() ) {
        return 'No entry to show.';
    }

    ob_start();
    ?>
    <table>
        <tr>
            <td>Name:</td>
            <td><?= esc_html( af_get_field( 'name' ) ) ?></td>
        </tr>
        <tr>
            <td>Email:</td>
            <td><?= esc_html( af_get_field( 'email' ) ) ?></td>
        </tr>
    </table>
    <?php
    return ob_get_clean();

} );
```

The 'thank you' page itself just needs a title and the `[afp_entry_summary]` shortcode — the form's [`redirect`](../display-arguments/#redirect) arg points at the page, and the visitor sees their submission summarised on arrival.

## Common gotchas

### The entry data isn't showing

If the form redirects to `/thank-you-page` (no trailing slash), WordPress will add a canonical redirect to `/thank-you-page/` — that's two redirects, and Advanced Forms only holds the submission for one. Always include the trailing slash when configuring the redirect URL.

### The entry data is from someone else's submission, or shows the fallback text

A page cache plugin is serving a cached copy of the thank-you page. Exclude the thank-you page from your caching layer (or any layer between it and the visitor) so each visitor sees their own submission.
