# How do I connect my form to an external service?

There are two paths: use one of the built-in integrations that ship with Advanced Forms Pro, or write a custom integration that POSTs the submitted data to the external service's API yourself.

## Built-in integrations

Advanced Forms Pro ships with integrations for **Slack**, **MailChimp**, and **Zapier**. Slack and MailChimp talk to those services directly. Zapier acts as a generic bridge — once a form is wired to a Zap, you can fan submissions out to the thousands of services Zapier supports without writing any code.

For MailChimp specifically, the UI exposes list, name, and email; tags and other subscriber fields are added via a filter. See [_Does the MailChimp integration support tags?_](./does-the-mailchimp-integration-support-tags/) for the pattern.

## Writing a custom integration

If your target service isn't covered by the built-ins (or by Zapier), hook into `af/form/submission`, read the field values with `af_get_field()`, and POST to the external API using `wp_remote_post()`.

```php
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    $payload = [
        'name'    => af_get_field( 'name' ),
        'email'   => af_get_field( 'email' ),
        'message' => af_get_field( 'message' ),
    ];

    $response = wp_remote_post( 'https://api.example.com/v1/leads', [
        'headers' => [
            'Content-Type'  => 'application/json',
            'Authorization' => 'Bearer ' . EXAMPLE_API_KEY,
        ],
        'body'    => wp_json_encode( $payload ),
        'timeout' => 15,
    ] );

    if ( is_wp_error( $response ) ) {
        error_log( 'External API submission failed: ' . $response->get_error_message() );
        return;
    }

    $code = wp_remote_retrieve_response_code( $response );

    if ( $code < 200 || $code >= 300 ) {
        error_log( 'External API returned ' . $code . ': ' . wp_remote_retrieve_body( $response ) );
    }

}, 10, 3 );
```

A note on user experience: the submission flow waits for your handler to finish before the user sees the success state. If the external API is slow or unreliable, that latency lands on your visitor. For anything non-trivial, queue the work with `wp_schedule_single_event()` (or a proper job queue) and let the submission return immediately — the user shouldn't wait on a third-party round trip.

## See also

- [Processing form submissions](./processing-form-submissions/) — the deeper guide on submission handlers, including custom handler patterns and where they fit in the lifecycle.
- [Does the MailChimp integration support tags?](./does-the-mailchimp-integration-support-tags/) — adding tags and other subscriber fields to the built-in MailChimp integration via PHP.
