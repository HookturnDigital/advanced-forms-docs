# Can I use Advanced Forms to enhance WooCommerce?

Advanced Forms has no built-in integration with WooCommerce. It can't be used to enhance or replace WooCommerce's own forms — the checkout, the account area, cart-related flows, and so on — out of the box.

That said, AF exposes a rich set of [actions and filters](Hooks-reference.md), so if you're prepared to write a bit of glue code you can use a form to collect information and then feed it into WooCommerce's data structures.

The most common pattern is to hook into [`af/form/submission`](Hooks-reference.md#afformsubmission) to read the submitted values with [`af_get_field()`](Functions-reference.md#afgetfield), and then call into WooCommerce — for example, attaching custom meta to an order being created via WooCommerce's own `woocommerce_checkout_create_order` action:

```php
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // Read whatever the form captured.
    $custom_note = af_get_field( 'custom_note' );

    if ( empty( $custom_note ) ) {
        return;
    }

    // Stash it somewhere WooCommerce can pick it up — e.g. the user's session,
    // a transient keyed to the user, or directly on the order if you're creating
    // one in this same request.
    WC()->session->set( 'af_custom_note', $custom_note );

}, 10, 3 );

add_action( 'woocommerce_checkout_create_order', function ( $order, $data ) {

    $custom_note = WC()->session->get( 'af_custom_note' );

    if ( ! empty( $custom_note ) ) {
        $order->update_meta_data( '_af_custom_note', $custom_note );
    }

}, 10, 2 );
```

This is illustrative — the right wiring depends on what you're actually trying to do. If the form runs *before* the WooCommerce checkout, session storage or a transient is usually the cleanest hand-off. If the form runs alongside or after an order exists, you can update the order meta directly inside `af/form/submission`.

The honest answer is "no integration, but you can wire something custom". Anything that's reachable from a WordPress action or filter is fair game.
