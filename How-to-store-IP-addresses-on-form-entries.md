# How to store IP addresses on form entries

Advanced Forms doesn't store the submitter's IP address by default — IP addresses are personal data under GDPR, and storing them without an explicit need is a compliance liability you don't want to inherit by default.

When you do need to store the IP (with the right user disclosure and legal basis in place), hook into [`af/form/entry_created`](https://advancedforms.github.io/actions/af/form/entry_created/) and write the IP onto the entry's post meta:

```php
add_action( 'af/form/entry_created', function ( $entry_id, $form ) {

    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    $ip = isset( $_SERVER['REMOTE_ADDR'] )
        ? sanitize_text_field( wp_unslash( $_SERVER['REMOTE_ADDR'] ) )
        : '';

    update_post_meta( $entry_id, '_af_entry_ip_address', $ip );

}, 10, 2 );
```

Reading from `$_SERVER['REMOTE_ADDR']` is fine when WordPress is fronted directly. Behind a load balancer or reverse proxy (Cloudflare, NGINX, AWS ALB), the visitor's real IP lives in a forwarded header (`HTTP_X_FORWARDED_FOR`, `HTTP_CF_CONNECTING_IP`, etc.) — use the appropriate header for your infrastructure and validate it carefully to avoid spoofing.

## Compliance reminder

If you're storing IPs, your privacy policy needs to disclose it and your data retention policy should specify how long they're kept and when they're deleted.
