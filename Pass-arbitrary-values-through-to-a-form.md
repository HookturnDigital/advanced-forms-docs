# Pass arbitrary values through to a form

When displaying a form, you can pass custom args to both the shortcode and the `advanced_form()` function. These args and their values are then available inside any custom hooked functions that receive the form's `$args`.

To avoid clashes with existing or future core form args, prefix your custom keys with something specific to your project — e.g. `xyz_user_name` instead of `user_name`.

## Passing values via the shortcode

```
[advanced_form form="YOUR_FORM_KEY_HERE" xyz_user_name="some value here"]
```

Shortcode values are static — you can't pass a dynamic PHP expression.

## Passing values via the function

```php
advanced_form( 'YOUR_FORM_KEY_HERE', [
    'xyz_user_name' => wp_get_current_user()->user_login,
] );
```

The function variant gives you full flexibility — the value can be anything you can compute in PHP.

## Reading the values in hooks

Custom args are available wherever the form's `$args` are passed, which is the case in most hook callbacks. For example, inside a custom submission handler:

```php
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    // Only run for a specific form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    if ( isset( $args['xyz_user_name'] ) ) {
        // Do something with $args['xyz_user_name'] …
    }

}, 10, 3 );
```

If you're in a context where the `$args` aren't provided directly, you can read them from the global AF instance instead:

```php
add_action( 'some_other_hook', function () {

    // AF()->submission is NULL when not processing a submission — guard for that.
    if ( AF()->submission !== null ) {
        $value = AF()->submission['args']['xyz_user_name'] ?? null;
    }

} );
```
