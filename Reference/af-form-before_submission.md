# `af/form/before_submission`

Action fired immediately before a form's submission handlers run. Use it to inspect the submitted values and **abort** the submission by calling `af_add_submission_error()` — the form is re-rendered with the error and submission handlers (post creation, user creation, notifications) do not run.

```php
add_action( 'af/form/before_submission', function ( $form, $fields, $args ) {
    // Block the submission based on a submitted value.
    if ( af_get_field( 'agreed_to_terms' ) !== '1' ) {
        af_add_submission_error( 'You must agree to the terms before continuing.' );
    }
}, 10, 3 );
```

The handler receives the `$form` array, the `$fields` array, and the `$args` array. The hook fires before [`af/form/submission`](../af-form-submission/) and before any built-in submission handlers (post-editing, user-editing, notifications, integrations). Calling `af_add_submission_error()` is the supported way to halt submission flow — returning early without calling it has no effect.

## Modifiers

- `af/form/before_submission` — Applies to all forms.
- `af/form/before_submission/key=FORM_KEY` — Applies to forms with a specific key.
- `af/form/before_submission/id=FORM_ID` — Applies to forms with a specific post ID.

## See also

- [How to prevent form submission based on submitted values](../prevent-form-submission-on-values/)
- [`af/form/submission`](../af-form-submission/) — Runs after this hook, once submission has been accepted.
