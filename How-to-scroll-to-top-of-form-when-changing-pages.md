# How to scroll to top of form when changing pages

On long multi-page forms, page transitions can leave the user mid-page with the new page's start above the fold. To smooth-scroll back to the top of the form on every page change, hook the [`af/form/page_changed`](https://advancedforms.github.io/actions/af/form/page_changed/) JavaScript action:

```js
jQuery(function ($) {

    acf.addAction('af/form/page_changed', function (page, previousPage, form) {

        const formSelector = '#' + form.key;

        $('html, body').animate({
            scrollTop: $(formSelector).offset().top,
        }, 'slow');

    });

});
```

The snippet applies to every Advanced Forms form on the page — gate on `form.key` if you only want the scroll behaviour for a specific form.

For an instant (non-animated) jump, swap the `$.animate(...)` call for `window.scrollTo(0, $(formSelector).offset().top)`.
