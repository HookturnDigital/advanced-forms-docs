# How to add a character count to Wysiwyg fields

ACF's WYSIWYG field is a wrapper around TinyMCE. To add a character counter beneath the toolbar — useful when you need to enforce a soft length cap on rich-text input — hook into ACF's TinyMCE settings filter and use TinyMCE's lifecycle events to update a counter element.

```php
add_action( 'af/form/after_fields', function () {
    ?>
    <script type="text/javascript">
        jQuery(function () {
            acf.addFilter('wysiwyg_tinymce_settings', function (mceInit) {

                mceInit.elementpath = false;

                mceInit.setup = function (ed) {

                    const characterLimit = 1000; // ← set your limit here.

                    let edContainer;
                    let countChar;
                    let colorOriginal;

                    ed.on('init', function () {
                        edContainer = jQuery(ed.iframeElement).closest('.acf-input');

                        countChar = edContainer
                            .find('.mce-statusbar .mce-flow-layout')
                            .prepend(
                                '<div class="mce-path mce-flow-layout-item mce-first count-char">' +
                                '<span class="count-char-current">0</span>' +
                                '<span>/' + characterLimit + '</span>' +
                                '</div>'
                            )
                            .find('.count-char-current');

                        colorOriginal = countChar.css('color');
                    });

                    const update = function () {
                        const node = ed.getBody();
                        const text = node.innerText || node.textContent;
                        const length = text.trim().length === 0 ? 0 : text.length;

                        countChar.css('color', length > characterLimit ? 'red' : colorOriginal);
                        countChar.text(length);
                    };

                    ed.on('change', update);
                    ed.on('keyup', update);
                };

                return mceInit;
            });
        });
    </script>
    <?php
} );
```

The counter shown is a soft cap — the user can still type past it, the count just goes red. To enforce a hard cap on submission, pair this with a [submission validator](./prevent-submission-by-value/) that checks the text length.
