# How to modify field values sent in notification emails

Field formatting in email notifications follows the field's **Return Format** setting in ACF — and there's no per-email override built in. The cleanest way to render a value differently in an email is to [register a custom merge tag](../working-with-merge-tags/) that resolves to the format you want, then use the merge tag in the notification's content.

For example: a radio field with **Return Format** set to **Both (Array)** produces output like `value, Label` in notifications. To display only the label, hook `af/merge_tags/resolve` and return the label slice of the array:

```php
add_filter( 'af/merge_tags/resolve', function ( $output, $tag ) {

    if ( $tag !== 'your_custom_merge_tag' ) {
        return $output;
    }

    // The radio field's return format is 'array', so af_get_field() returns
    // ['value' => …, 'label' => …]. Return the label only.
    $value = af_get_field( 'your_radio_field_name' );

    return $value['label'] ?? '';

}, 10, 2 );
```

Use the merge tag in the notification's **Content** field as `{your_custom_merge_tag}`.

This pattern generalises to any field type — wrap the read in `af_get_field()`, transform it into the format you want, and return that from the merge-tag resolver.
