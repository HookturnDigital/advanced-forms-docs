# `af/form/page_changed`

This Javascript action will be triggered when the current page is changed in a multi-page form.

```js
acf.addAction( 'af/form/page_changed', function( newPage, previousPage, form ) {
    console.log("Changed page from %d to %d", previousPage, newPage);
});
```

## Scroll to top of form when page changes

```js
acf.addAction( 'af/form/page_changed', function( newPage, previousPage, form ) {
    $( 'html, body' ).animate({
        scrollTop: form.$el.offset().top,
    }, 1000);
});
```
