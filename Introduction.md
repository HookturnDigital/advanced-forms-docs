# Introduction

## What is this plugin?

The [Advanced Forms](https://hookturn.io/downloads/advanced-forms-pro/) plugin is an extension for
the [Advanced Custom Fields Pro](https://www.advancedcustomfields.com/pro/)
plugin. It allows you to create forms in the WordPress admin which can load one or more ACF field groups on the front
end of a website as a front end ACF form. The plugin provides many useful features such as:

- **Email notifications:** Send any number of emails to multiple recipients with custom content.
- **AJAX submissions:** Submit forms without reloading the page.
- **Saved entries:** Save form submissions to the database as a built-in _entry_ post type.
- **Simple spam protection:** Protect your ACF forms from spam with the built-in honeypot field.
- **Restricted access:** Control access to a form based on number of entries, user logged in status, or set a date range
  for form availability.
- **Multi-step forms:** Break down complex ACF forms into multiple steps.
- **UI or code based form creation:** Create forms either through the admin panel or programmatically.
- **Import/export forms:** Easily import and export ACF forms to share between sites.
- **Gutenberg support:** Use the Advanced Forms block to add forms to your posts and pages in the block editor.
- **Developer friendly:** On top of ACF's powerful API, Advanced Forms provides a number of hooks and filters to extend
  the plugin's functionality.

### Pro features

In addition to the core features available in the free version of the plugin, the pro version includes:

- **Post creation and editing:** Create and edit posts from the front end of your site.
- **User creation and editing:** Create and edit users from the front end of your site.
- **Calculated fields:** Perform AJAX-based calculations on form fields and display any HTML output in the form.
- **Slack integration:** Send form submissions to a Slack channel.
- **Mailchimp integration:** Subscribe users to a Mailchimp list on submission.
- **Google reCaptcha integration:** Protect your forms from spam with Google's reCaptcha service.
- **Zapier integration:** Send form submissions to thousands of other services using Zapier.

## Basic requirements

The basic requirements for the plugin are as follows:

- ACF PRO version 5 or greater.
- WordPress version 5.4.0 or greater.
- PHP version 7.0 or greater.

**Note:** the plugin may work on older versions but we do develop for modern versions so it is best to remain updated at
all times.

## Supported field types

All core field types including repeaters, groups, and flexible content fields are supported. Generally speaking, any
field type should work with Advanced Forms but there may be situations where a third-party field type doesn't function
as expected due to differences in how Advanced Forms renders field groups on the front end. If you find a field type
that isn't supported, please [let us know](mailto:support@hookturn.io?subject=AF%20Unsupported%20field%20type).