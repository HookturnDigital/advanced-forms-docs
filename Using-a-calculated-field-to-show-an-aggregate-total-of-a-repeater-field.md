# Using a calculated field to show an aggregate total of a repeater field

Aggregating values across repeater rows isn't possible with a pure JavaScript calculated field — calculated fields can't be used *inside* a repeater, and the JS calculator doesn't have visibility into the repeater's rows. The workaround is to switch to a PHP-based calculator: hook `af/field/calculate_value/name=<calculated-field-name>` and walk the repeater rows server-side.

The example below sums a `number` sub-field across every row of a `data_rows` repeater into a `total` calculated field:

```php
$calculated_field_name = 'total';

add_filter( 'af/field/calculate_value/name=' . $calculated_field_name, function () {

    $sum = 0;

    // af_get_field() on a repeater returns a 2D array of rows × sub-fields.
    $rows = af_get_field( 'data_rows' );

    if ( is_array( $rows ) ) {
        foreach ( $rows as $row ) {

            $value = $row['number'] ?? 0;

            // Skip empty/non-numeric values so we don't get warnings.
            if ( ! is_numeric( $value ) ) {
                continue;
            }

            $sum += (float) $value;
        }
    }

    return 'The total is: ' . $sum;

}, 10, 0 );
```

Because this is a server-side calculation, each update round-trips to PHP — see [How to trigger a calculated field update](./trigger-calculated-field-update/) for caveats on request volume and debouncing if you want the total to update live as the user types.
