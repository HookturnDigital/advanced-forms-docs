# Hotfix for multiple forms on same page triggering validation across all fields

**Symptom:** With ACF 5.11+ and more than one Advanced Forms form on the same page (each with at least one required field), submitting one form runs validation against required fields in the *other* form too — so the submission fails on fields the user never interacted with.

**Cause:** ACF 5.11 introduced a validation routine (`ensureInvalidFieldVisibility`) that scans every `.acf-field input` on the page rather than scoping to the form being submitted. There's an open issue on ACF Pro's repo; this snippet is a hotfix until that's resolved upstream.

## Step 1 — JavaScript hotfix

Save the following as `af-acf-validation-hotfix.js` somewhere your theme can serve it (e.g. `assets/js/` in your theme):

```js
(function ($) {

    // From ACF 5.11 — unchanged.
    var ensureFieldPostBoxIsVisible = function ($el) {
        var $postbox = $el.parents('.acf-postbox');
        if ($postbox.length) {
            var acf_postbox = acf.getPostbox($postbox);
            if (acf_postbox && acf_postbox.isHiddenByScreenOptions()) {
                acf_postbox.$el.removeClass('hide-if-js');
                acf_postbox.$el.css('display', '');
            }
        }
    };

    // From ACF 5.11, modified to scope to the submitting form rather than the whole page.
    var ensureInvalidFieldVisibility = function ($form) {
        $form.find('.acf-field input').each(function () {
            if (!this.checkValidity()) {
                ensureFieldPostBoxIsVisible($(this));
            }
        });
    };

    // Override ACF's onClickSubmit to find the current form and pass it as scope.
    acf.validation.onClickSubmit = function (e, $el) {
        var $form = $el.closest('form');
        if (!$form.length) return;

        ensureInvalidFieldVisibility($form);
        this.set('originalEvent', e);
    };

})(jQuery);
```

## Step 2 — Enqueue on form render

```php
add_action( 'af/form/enqueue', function () {

    // Adjust the path to match where you saved the file.
    $url = get_stylesheet_directory_uri() . '/assets/js/af-acf-validation-hotfix.js';

    wp_register_script( 'af-acf-validation-hotfix', $url, [ 'acf-input' ] );
    wp_enqueue_script( 'af-acf-validation-hotfix' );

} );
```

The hotfix only runs on pages where Advanced Forms is enqueueing its own scripts (`af/form/enqueue`), so it has no effect outside the forms it's meant to fix.

Remove the hotfix once ACF resolves the upstream issue.
