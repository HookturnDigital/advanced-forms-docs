# How to trigger a form to save via AJAX as the form is updated

When you combine the [`ajax`](https://advancedforms.github.io/guides/available-arguments/#ajax) form arg with [`filter_mode`](https://advancedforms.github.io/guides/available-arguments/#filter_mode) and [post editing](https://advancedforms.github.io/guides/creating-and-editing-posts/#editing-existing-posts), you get a form that stays in place and keeps updating the same post without ever showing a success message.

You can take that one step further: hide the submit button and trigger submissions automatically as the form is updated.

```js
// Drop this into a <script> tag in your site's footer, or a custom JS file
// enqueued on pages that render the form.

jQuery(function () {

    acf.addAction('af/form/setup', function (form) {

        // Limit to a specific form.
        if (form.key !== 'form_62d646deb2888') {
            return;
        }

        // Hide the submit button.
        form.$el.find('.af-submit').hide();

        // Submit on every change event. For text-based fields this only fires
        // when the user tabs/clicks out of the field.
        form.$el.on('change', function () {
            form.$el.trigger('submit');
        });

        // Optional: also submit on keystrokes for live updates as the user types.
        // WARNING: this fires a lot of requests and can cause race conditions where
        // a slower earlier update overwrites a newer one. If you go this route,
        // debounce the submission — see
        // https://www.freecodecamp.org/news/javascript-debounce-example/.
        form.$el.on('keyup', function () {
            form.$el.trigger('submit');
        });

    });
});
```

This is resource-intensive — every change is a full AJAX request to the server. Reach for it only when the UX genuinely warrants it.
