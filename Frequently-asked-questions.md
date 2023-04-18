# Frequently asked questions

## Can I have multiple forms on the same page?

Yes, you may have multiple forms on the same page.

## Is it possible to handle submissions without a page reload (using AJAX)?

Advanced Forms can handle submissions without a reload using AJAX. Activate it for your form using
the [`ajax` display argument](Display-arguments.md#uploader).

## Why does the image/file upload field change when a user is logged in or not?

This discrepancy is due to how ACF handles image uploads in response to WordPress. When a user is not logged or lacks
the relevant permissions (typically the `upload_files` capability), the default/basic HTML upload field is rendered
without the necessary scripts to stylize it and use the WP media uploader. This is built into ACF's field rendering
logic and it aligns with WordPress' core requirements for using the media library.

The basic HTML upload field does render differently to the stylised version which is why they appear different and
require their own CSS to style. You may wish to add some custom CSS for the basic HTML upload field as per [this example
on CSS-Tricks](https://css-tricks.com/snippets/css/custom-file-input-styling-webkitblink/).

If you prefer to use the basic HTML uploader for a consistent experience for all users, you can configure this in the
form args by setting the `uploader` display option to `basic`. This will disable the media library UI but doesn't
require special permissions or a logged in status. See [_Display options_](Display-arguments.md#uploader) for examples
on how to do so with both the shortcode and the`advanced_forms()` function.

## Why is my success message not showing after submission?

This issue is often caused by page caching provided either by your host or a plugin on your site. We recommend disabling
the cache for the page with your form or to create a custom “thank you” page and redirecting to that after submission.

## I’m seeing weird behaviour with my form that has a large number of fields. What could be wrong?

If your form has a very large number of fields there is a risk that you will run into limits set by PHP. If the form is
not working at all or you’re seeing weird behaviour, we recommend increasing the max_input_vars setting in PHP.

## I can’t upload images using a gallery field in a form

ACF Gallery fields use the WP media uploader under the hood which means that they require a signed in user to work. The
user must also have the right capabilities to access the media library. If you want your form to be accesible for
non-signed-in users as well, we recommend replacing the gallery field with a repeater of image fields.

## Can I use Advanced Forms to list a user's posts for editing or deleting?

Using Advanced Forms Pro, a form can be used to both create and edit posts but Advanced Forms only handles the form
itself. Displaying lists of posts is a bit outside the scope of the plugin but can be achieved a number of different
ways. The right way for your project will depend on what kind of theme setup you have in place. e.g; if you have a
custom coded theme, you have the flexibility of writing queries and template loops to suit the situation but if you are
using a page builder, you may have to adapt using the tools provided.

The basic idea, however, is simple: you simply need to configure your theme to show all posts which the user is an
author for (Advanced Forms Pro will automatically make them the author when creating a post) and then on each individual
post page you can use the [[advanced_form] shortcode](TODO) to render a form for editing the current post. TODO - link
to a guide on how to do this

Alternatively, you may wish to have a single dedicated page with your form and use a custom URL parameter to pass a post
ID to the form.
See [this guide](Creating-and-editing-posts.md#using-a-url-parameter-to-control-which-post-is-being-edited) for an
example on how you can achieve this result.

### What about deleting posts?

Again, this is something you'll need to build into your theme/implementation but consider using WordPress'
[get_delete_post_link() function](https://developer.wordpress.org/reference/functions/get_delete_post_link/) within a
query loop.

## Can I set the license key using WP CLI?

No, this is not currently possible but we may look at providing this down the track.

## Can I export form entries?

There isn't currently any built in mechanism for exporting form entries but entries are stored as posts with their own
ACF field data so this can be done using tools specifically designed to export data from WordPress.

[WP All Export](https://wordpress.org/plugins/wp-all-export/) is a fantastic option for exporting posts.

## Can I generate a PDF from a form submission?

PDF generation is not a built-in feature of the plugin at this stage. However, you may write your own code to hook into
the form submission, generate a PDF, and attach the PDF to the form using
the [af/form/email/attachments filter](Hooks-reference.md#afformemailattachments). The
basic process will be as follows:

1. Generate a PDF via PHP with field values retrieved using [af_get_field()](Functions-reference.md#afgetfield).
2. Save the PDF to a file on the server.
3. Return the file path from the filter to attach it to the email.

## Can I use Advanced Forms to enhance WooCommerce?

Advanced Forms has no integration with WooCommerce at this time so it cannot be used to enhance any WooCommerce forms
such as checkout, account management, etc.

The plugin does offer a rich set of actions and filters so you may well be able to get creative and use it to collect
information that you then inject into WooCommerce data structures.

## Can I display the user's password in a form field?

No, it isn't possible to display a user's password. This is because WordPress does not store passwords in plain text but
instead, stores them as a hash which cannot be reversed. This helps to prevent the risk of passwords being leaked when a
hacker gains access to the database.

## Does the MailChimp integration support tags?

The integration only supports list, name, and email address through the UI. However, you may use the
[af/form/mailchimp/request filter](Hooks-reference.md#afformmailchimprequest) to modify the API request as needed.

To learn more about what can be achieved via the MailChimp API this way, see
the ["Add member to list" endpoint reference here](https://mailchimp.com/developer/marketing/api/list-members/add-member-to-list/).

## Can users access the media library via Advanced Forms?

If a user is logged in and has permission to access the media library, upload fields should default to using the media
library. This can be configured using the [`uploader` display argument](Display-arguments.md#uploader).

### Limiting available media

Restrictions on available media is best handled on the WordPress level rather than within Advanced Forms. There are some
guides online on how to achieve this as well as some plugins which can help.
See [this article](https://www.greengeeks.com/tutorials/protect-media-library-wordpress-users-uploads/) for both plugin-
and code-based examples.

## How do I redirect to another page after submission?

You can pass a URL to the [`redirect` display argument](Display-arguments.md#redirect) when displaying a form.

## How do I change the submit button text?

You can pass custom submit button text to the [`submit_text` display argument](Display-arguments.md#submittext) when
displaying a form.

## How do I connect my form to an external service?

Advanced Forms Pro has built-in integrations with Slack, Mailchimp, and thousands of other services through Zapier. If
you need to integrate with a service that is not supported it’s normally simple to build a custom integration. This
normally entails writing a [custom submission handler](Processing-form-submissions.md#custom-submission-handlers) which
collects form data and sends it to an third-party API.

## How do I only subscribe users in Mailchimp if they have checked a checkbox?

To achieve this you can use the [af/form/mailchimp/request filter](Hooks-reference.md#afformmailchimprequest) and
conditionally return `false` to stop the request. For example, if you have a checkbox field named
`subscribe_to_newsletter` you can use the following snippet:

```php
<?php
add_filter( 'af/form/mailchimp/request', function ( $request, $form, $args ) {
	
	// Isolate to a specific form.
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

## How do I remove the default styles for a form?

Advanced Forms will enqueue default styles from both ACF and its own styles. To dequeue all of them, use the following
snippet:

```php
<?php
add_action( 'af/form/enqueue/key=FORM_KEY', function ( $form, $args ) {
	// Remove default Advanced Forms styles
	wp_dequeue_style( 'af-form-style' );
	
	// Remove default ACF styles
	wp_dequeue_style( 'acf-input' );
	wp_dequeue_style( 'acf-pro-input' );
} );
```

