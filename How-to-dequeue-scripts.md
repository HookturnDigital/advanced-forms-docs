# How to dequeue scripts

Advanced Forms enqueues two scripts when rendering a form — `af-forms-script` (Advanced Forms' own) and `acf-input` (ACF's). Both depend on jQuery, which gets pulled in automatically.

The official guide covers the supported way to dequeue them via `af/form/enqueue`:
[Decreasing the number of scripts and styles](https://advancedforms.github.io/guides/decreasing-number-of-scripts-and-styles/).

## Tread carefully

Most forms genuinely need jQuery — ACF uses it for field validation, the WYSIWYG, the date/colour pickers, the media library integration, and most field-type behaviour. Dequeuing it (or anything else Advanced Forms / ACF depend on) tends to break form behaviour in non-obvious ways.

The right reason to dequeue is when *another* enqueue on your site already provides the dependency and you're trying to avoid the duplicate. The wrong reason is "to make the page lighter" — the breakage cost generally outweighs the bytes saved.
