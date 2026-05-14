# How to assign taxonomy terms to created/edited posts

When mapping taxonomy fields to a created or edited post, the ACF field gets updated but the terms aren't assigned to the post itself. This is a known limitation — until it's resolved in core, you can bridge the gap with the snippet below.

```php
// Apply the terms from a 'categories' ACF field to the post after it's created.
add_filter( 'af/form/editing/post_created', 'example_map_taxonomy_terms_to_post', 10, 3 );

// To do the same on edit, also hook into the post-updated filter:
// add_filter( 'af/form/editing/post_updated', 'example_map_taxonomy_terms_to_post', 10, 3 );

function example_map_taxonomy_terms_to_post( $post, $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // The post is already saved at this point, so we can read the ACF value
    // directly off it and use that to set the terms.
    if ( $terms = get_field( 'categories', $post->ID ) ) {

        // If the field returns term objects, extract the IDs. If it returns an
        // array of term IDs already, you can use $terms directly below.
        $term_ids = [];
        foreach ( $terms as $cat ) {
            $term_ids[] = $cat->term_id;
        }

        wp_set_post_terms( $post->ID, $term_ids, 'category' );
    }
}
```

Adjust the field name (`categories`), taxonomy (`category`), and form key for your setup.
