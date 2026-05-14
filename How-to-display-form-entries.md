# How to display form entries

Advanced Forms stores entries as a custom post type (`af_entry`). ACF field values are saved against the entry post via [`update_field()`](https://www.advancedcustomfields.com/resources/update_field/), and the source form's key is stored in postmeta under `entry_form`.

Because it's all standard WordPress data, you can use normal `WP_Query` / `get_posts()` calls and ACF's template functions to surface entries anywhere on the front end.

The following example queries every entry for a given form and renders the field values as an HTML table:

```php
<?php
// Query entries for a specific form via the entry_form postmeta key.
$entries = get_posts( [
    'post_type'      => 'af_entry',
    'posts_per_page' => -1,
    'meta_query'     => [
        [
            'key'   => 'entry_form',
            'value' => 'form_63603dfd5f7d2',
        ],
    ],
] );

if ( $entries ): ?>
    <table class="entries">
        <tr>
            <th>Entry ID</th>
            <th>Entry #</th>
            <th>First Field</th>
            <th>Second Field</th>
        </tr>
        <?php foreach ( $entries as $entry ): ?>
            <tr>
                <td><?= $entry->ID ?></td>
                <td><?= get_the_title( $entry ) ?></td>
                <td><?php the_field( 'first_field', $entry->ID ); ?></td>
                <td><?php the_field( 'second_field', $entry->ID ); ?></td>
            </tr>
        <?php endforeach; ?>
    </table>
<?php endif; ?>
```

Replace `form_63603dfd5f7d2` with the form key from your own form, and the field names (`first_field`, `second_field`) with the field names you want to render.
