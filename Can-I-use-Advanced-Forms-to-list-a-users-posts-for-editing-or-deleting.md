# Can I use Advanced Forms to list a user's posts for editing or deleting?

Advanced Forms Pro can create and edit posts via a form, but the plugin only handles the form itself — it doesn't provide any UI for listing or deleting posts. Those pieces need to be built into your theme. How you do that depends on your setup: a custom-coded theme gives you full flexibility with `WP_Query` and template loops, while a page builder will mean working with the tools that builder provides.

The standard pattern is straightforward. Configure your theme to show all posts where the current user is the author — Advanced Forms automatically sets the submitting user as the author when a form creates a post — and then on each individual post template, use the [`[advanced_form]` shortcode](./displaying-a-form/) to render a form that edits the current post. See [_Creating and editing posts_](./creating-and-editing-posts/) for the editing flow.

Alternatively, you can have a single dedicated edit page with one form on it, and pass the post ID via a URL parameter (e.g. `?post_id=123`) to tell the form which post to edit. Your listing template links each post to that edit page with the appropriate parameter. This keeps the editing UI in one place rather than scattered across every single-post template.

## What about deleting posts?

Same story — you'll need to build it into your theme. The simplest approach is WordPress' [`get_delete_post_link()`](https://developer.wordpress.org/reference/functions/get_delete_post_link/) inside your query loop. It generates a nonce-protected URL that deletes the post when followed, and you can pass a specific post ID if you're rendering the link outside the loop. Wrap it in a confirmation step on the front end if you don't want accidental deletions.
