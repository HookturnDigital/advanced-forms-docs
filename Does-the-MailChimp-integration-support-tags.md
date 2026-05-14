# Does the MailChimp integration support tags?

Yes — tags can be added via PHP. The MailChimp integration's UI only exposes list, name, and email address, but the [`af/form/mailchimp/request`](../af-form-mailchimp-request/) filter lets you modify the outgoing API request before it's sent, so you can append tags (or any other field MailChimp's API supports) programmatically.

The full PHP example lives in the MailChimp doc: [_How to add tags using PHP_](./mailchimp/#how-to-add-tags-using-php).

For the full set of fields you can attach to a subscriber via this filter, see MailChimp's ["Add member to list" endpoint reference](https://mailchimp.com/developer/marketing/api/list-members/add-member-to-list/).
