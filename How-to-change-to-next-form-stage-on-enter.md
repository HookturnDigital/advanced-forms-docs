# How to change to next form stage on enter

By default, pressing Enter inside a form submits it. On a multi-stage form that's usually wrong — Enter should advance to the next stage, not submit the whole form. The following snippet intercepts the Enter key and clicks the form's **Next** button instead:

```js
jQuery(function ($) {

    // Set this to your form's key.
    const formKey = 'YOUR_FORM_KEY_HERE';

    $('.af-form input').on('keydown', function (e) {
        if (e.originalEvent.key === 'Enter') {
            e.preventDefault();

            if (af.forms[formKey] && af.forms[formKey].$next_button) {
                af.forms[formKey].$next_button.click();
            }

            return false;
        }
    });

});
```

If your form has a textarea, the snippet above will also block Enter inside it — adjust the selector (`'.af-form input'`) to exclude textareas or any other field where Enter should still work as line-break:

```js
$('.af-form input:not([type="submit"]), .af-form select').on('keydown', …);
```
