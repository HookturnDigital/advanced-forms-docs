# Customising the form success message

The default success message is set in **Form Settings > Display > Success message** in the form editor. When you need the message to vary by form, by user, or by what happened during submission, filter it in PHP with [`af/form/success_message`](https://advancedforms.github.io/filters/af/form/success_message/).

## Basic usage

Override the success message for a specific form by checking the form key:

```php
add_filter( 'af/form/success_message', function ( $success_message, $form, $args ) {

    if ( $form['key'] === 'YOUR_FORM_KEY_HERE' ) {
        $success_message = 'Thanks — we have received your submission.';
    }

    return $success_message;

}, 10, 3 );
```

## Setting the message from a submission handler

A more interesting use case: you're doing work during submission — sending data to a third-party API, scoring a quiz, validating against an external system — and you want the success message to reflect the result.

The trick is that `AF()->submission` persists across the lifetime of a single submission. Stash whatever you need during the submission action, then read it back in the `af/form/success_message` filter:

```php
// During submission, run your custom logic and stash the result.
add_action( 'af/form/submission', function ( $form, $args ) {

    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // Example: score a quiz, call an external API, etc.
    $score = (int) get_field( 'quiz_score' );
    $passed = $score >= 70;

    // Prefix custom keys (e.g. xyz_) to avoid clashes with future core fields.
    AF()->submission['xyz_quiz_passed'] = $passed;
    AF()->submission['xyz_quiz_score']  = $score;

}, 10, 2 );

// Read the stashed result back when the success message is rendered.
add_filter( 'af/form/success_message', function ( $success_message, $form, $args ) {

    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return $success_message;
    }

    if ( ! isset( AF()->submission['xyz_quiz_passed'] ) ) {
        return $success_message;
    }

    $passed = AF()->submission['xyz_quiz_passed'];
    $score  = AF()->submission['xyz_quiz_score'];

    if ( $passed ) {
        return "Nice work — you scored {$score}%. We'll be in touch.";
    }

    return "You scored {$score}%. Have another go when you're ready.";

}, 10, 3 );
```

The same pattern works for anything you can compute or fetch during submission: API responses, validation outcomes, eligibility checks, conditional thank-you messages.

## See also

- [How to dynamically change a success message when a post is updated](./how-to-dynamically-change-a-success-message-when-a-post-is-updated/) — the same `AF()->submission` stash-and-read pattern, scoped to post-update submissions.
