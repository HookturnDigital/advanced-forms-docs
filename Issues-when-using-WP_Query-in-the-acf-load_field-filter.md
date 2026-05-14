# Issues when using WP_Query in the `acf/load_field` filter

**Symptom:** A form's edit screen fails to load any field groups, and the error log fills with warnings like:

```
PHP Warning:  Trying to access array offset on value of type bool in /wp-content/plugins/advanced-forms/admin/admin-forms.php on line 117
```

**Cause:** A `WP_Query` instantiated inside an [`acf/load_field`](https://www.advancedcustomfields.com/resources/acf-load_field/) callback is overriding core globals (`$post`, `$wp_query`) that Advanced Forms relies on while rendering the form edit screen. The query loop leaves those globals in a partially-modified state, and downstream code that assumes they reflect the actual current request breaks.

**Fix:** Swap `WP_Query` for `get_posts()` — it doesn't mutate the globals.

```php
add_filter( 'acf/load_field/key=YOUR_FIELD_KEY_HERE', function ( $field ) {

    $field['choices'] = [];

    // Use get_posts() rather than WP_Query — get_posts() doesn't touch the
    // global $post / $wp_query, which is what was tripping the AF edit screen.
    $posts = get_posts( [
        'post_type'      => 'post',
        'posts_per_page' => -1,
        'orderby'        => 'title',
        'order'          => 'ASC',
    ] );

    foreach ( $posts as $post ) {
        $field['choices'][ $post->ID ] = get_the_title( $post );
    }

    return $field;

} );
```

If you genuinely need `WP_Query` (e.g. you're using `tax_query` features `get_posts()` doesn't directly support, or you want pagination), wrap the loop with `wp_reset_postdata()` after iterating and don't use `setup_postdata()` mid-loop — that's the source of the global pollution.

The same fix applies anywhere ACF's filters are running in a context where the globals matter — `acf/prepare_field`, `acf/fields/relationship/query`, etc.
