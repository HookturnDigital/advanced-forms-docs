# How to show a notice above the form when using filter mode

When using **filter mode** to keep a form in place after submission (e.g. for a profile-edit form), Advanced Forms doesn't render a confirmation message by default. Add one yourself using [`af/form/before_title`](../af-form-before_title/):

```php
add_action( 'af/form/before_title', function ( $form, $args ) {

    // Only run for the target form.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    // af_has_submission() is true only when we just processed one — perfect for
    // showing a one-time confirmation message on the post-submit render.
    if ( af_has_submission() ) {
        ?>
        <div class="form-update-notice">
            Your changes have been saved.
        </div>
        <?php
    }

}, 10, 2 );
```

Several `af/form/before_*` and `af/form/after_*` hooks exist if `before_title` isn't the right placement — see [Customizing how the form renders](../customizing-how-the-form-renders/) for the full list.

## AJAX-submitted forms

The PHP-side notice above only renders on a full-page submission. With AJAX, the form UI isn't rebuilt, so PHP doesn't run again and the notice never appears. For AJAX forms, hook the [`af/form/ajax/submission`](https://github.com/advancedforms/advanced-forms/blob/main/assets/js/forms.js) JavaScript hook and inject the notice from JS instead.

A common pattern is to use both — the PHP notice for full-page submissions, and matching JS for AJAX submissions — so the UX is consistent regardless of which submission style is in play.
