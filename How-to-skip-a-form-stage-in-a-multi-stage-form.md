# How to skip a form stage in a multi-stage form

Skip a stage when it isn't relevant to the current submission — for example, jumping past a "Shipping address" stage when the user has selected "Pickup", or auto-advancing past a stage whose fields are all hidden by conditional logic. Multi-stage forms expose a JavaScript action that fires on every page change, which is where the skip happens.

## Skip a stage based on a custom condition

Hook into the `af/form/page_changed` JS action and call `af.pages.changePage()` to jump to a different page. Page numbers are zero-indexed.

```php
add_action( 'af/form/enqueue', function ( $form, $args ) {

    // Limit this script to one specific form — remove this guard to apply it everywhere.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    ?>
    <script>
        jQuery(function ($) {

            acf.addAction('af/form/page_changed', function (pageTo, pageFrom, form) {

                // When the user lands on the second stage (index 1)…
                if (pageTo === 1) {

                    // …read a field value from the form to decide whether to skip.
                    // Replace `your_field_name` with the ACF field name. ACF renders each
                    // field inside a wrapper element with a matching `data-name` attribute.
                    const choice = form.$el
                        .find('[data-name="your_field_name"]')
                        .first()
                        .find('input, select, textarea')
                        .first()
                        .val();

                    // Skip the second stage when the user picked "pickup".
                    if (choice === 'pickup') {
                        // Jump to stage 3 (index 2).
                        af.pages.changePage(2, form);
                    }
                }
            });

        });
    </script>
    <?php
}, 10, 2 );
```

A few things worth noting:

- `pageTo` is the stage the form is moving *to*; `pageFrom` is the stage it's leaving. Both are zero-indexed.
- `af.pages.changePage( index, form )` triggers another `af/form/page_changed` call for the new stage, so any skip logic for the target stage will also run — handy when you need to chain skips.
- Stage indices are based on the form's stage configuration, not on what's currently visible. Hidden stages still take a slot in the index.

## Skip a stage whose fields are all hidden

If a stage's fields are all hidden by ACF's conditional logic, that stage will appear blank to the user. Auto-advance past it by checking whether any fields on the new stage are visible:

```php
add_action( 'af/form/enqueue', function ( $form, $args ) {

    // Limit this to one form — remove the guard to apply it everywhere.
    if ( $form['key'] !== 'YOUR_FORM_KEY_HERE' ) {
        return;
    }

    ?>
    <script>
        jQuery(function ($) {

            acf.addAction('af/form/page_changed', function (pageTo, pageFrom, form) {

                // Grab every field on the stage we just moved to.
                const currentPageFields = form.pages[pageTo].$fields;

                // Are any of them actually visible?
                const pageHasVisibleFields = !!currentPageFields.filter(':visible').length;

                // Detect the direction of travel so we skip the right way.
                const isForward = pageTo > pageFrom;

                if (!pageHasVisibleFields) {
                    const nextPage = isForward ? pageTo + 1 : pageTo - 1;
                    af.pages.changePage(nextPage, form);
                }
            });

        });
    </script>
    <?php
}, 10, 2 );
```

This snippet honours the direction the user was going — moving forward through the form skips forward, and clicking "Previous" skips backward — so the user can navigate naturally without getting trapped on a blank stage.

## Why hook into `af/form/enqueue` rather than printing inline?

`af/form/enqueue` only fires when the form is actually being rendered on the page, so the script doesn't load site-wide. If you only target one form, the early `$form['key']` check keeps the script tight to that form too.

## See also

- [Working with multi-stage forms](./working-with-multi-stage-forms/) — the broader topic doc covering how multi-stage forms are configured and the lifecycle actions they expose.
