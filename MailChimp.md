## How to add tags using PHP

The integration only supports list, name, and email address through the UI. However, you may use the
[af/form/mailchimp/request filter](Hooks-reference.md#afformmailchimprequest) to modify the API request as needed.

[//]: # (todo - add example here)

To learn more about what can be achieved via the MailChimp API this way, see
the ["Add member to list" endpoint reference here](https://mailchimp.com/developer/marketing/api/list-members/add-member-to-list/).

## How to use add an opt-in checkbox for MailChimp

To achieve this, add a checkbox field to your ACF field group that will act as an opt in. Then you may use
the [af/form/mailchimp/request filter](Hooks-reference.md#afformmailchimprequest) and conditionally return `false` to
stop the request if the checkbox is not checked.

For example, if the form contains a checkbox field named `subscribe_to_newsletter` you can use the following snippet:

```php
<?php
add_filter( 'af/form/mailchimp/request', function ( $request, $form, $args ) {
	
	// Isolate to a specific form by key.
	if( $form['key'] !== 'form_62bd15508b9c9' ){
		return $request;
	}

	// If the checkbox is not checked, return false to stop the request.
	if ( ! af_get_field( 'subscribe_to_newsletter' ) ) {
		return false;
	}
	
	return $request;
}, 10, 3 );
```