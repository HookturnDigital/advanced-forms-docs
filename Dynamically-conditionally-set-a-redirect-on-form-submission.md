# Dynamically / conditionally set a redirect on form submission

To override a form's redirect at submission time — for example, sending users to different pages based on what they entered — hook into [`af/form/submission`](../af-form-submission/) and write the URL onto the submission's `redirect` arg.

The trick is **not** to mutate the form object, but to mutate the args on the current submission:

```php
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    $value = af_get_field( 'some_field_name' );

    if ( $value ) {
        AF()->submission['args']['redirect'] = '/some/page?data=' . urlencode( $value );
    }

}, 20, 3 );
```

The priority of 20 ensures this runs after Advanced Forms' own submission handler (which is on priority 10) so the redirect override sticks.

## See also

- [How to redirect the form submission after post creation](./redirect-after-post-creation/) — overrides the redirect using the new post's ID.
- [How to redirect the form submission after user creation](./redirect-after-user-creation/)
