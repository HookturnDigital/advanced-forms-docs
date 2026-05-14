# How to use a Google Map field address in emails & success messages

ACF's Google Map field stores its value as a JSON-encoded string with multiple keys (`address`, `lat`, `lng`, `zoom`, `place_id`, etc.). When that field is referenced via a merge tag in an email or success message, the raw escaped JSON gets rendered — not what you want.

Register a custom merge tag that resolves to just the slice of the payload you care about (the address, in the example below):

```php
add_filter( 'af/merge_tags/resolve', function ( $output, $tag ) {

    if ( $tag !== 'google_map_address' ) {
        return $output;
    }

    $value = af_get_field( 'google_map' );

    if ( empty( $value ) ) {
        return '';
    }

    // The value is an escaped JSON string — un-slash, then decode.
    $value = json_decode( wp_unslash( $value ), true );

    return $value['address'] ?? '';

}, 10, 2 );
```

Use the merge tag in the notification's content (or success message) as `{google_map_address}`.

Other useful keys on the same value: `lat`, `lng`, `name`, `place_id`, `street_number`, `street_name`, `city`, `state`, `country`, `post_code`. Register additional merge tags following the same pattern for any of them you need.
