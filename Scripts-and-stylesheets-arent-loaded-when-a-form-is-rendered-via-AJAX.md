# Scripts & stylesheets aren't loaded when a form is rendered via AJAX

**Symptom:** A form renders fine on a normal page load but appears unstyled / non-interactive when injected into a page via AJAX — fields don't initialise, validation doesn't trigger, the wysiwyg / date picker / media-library buttons don't work.

**Cause:** Advanced Forms enqueues its scripts and styles during normal form rendering. When the form is rendered via AJAX, that enqueueing never happens — the request that built the form's HTML doesn't go through the normal enqueue lifecycle.

**Fix:** Call `af_enqueue()` so Advanced Forms registers its assets even when the form's markup is being assembled out-of-band.

Two common places to call it from:

1. **From a template that renders the form via AJAX** — call it directly where the form will eventually be inserted:

   ```php
   <?php af_enqueue(); ?>
   ```

2. **From a `wp_enqueue_scripts` callback** — when you know the page is going to inject an AF form later (e.g. a modal):

   ```php
   add_action( 'wp_enqueue_scripts', function () {
       if ( is_page( 'your-modal-page-slug' ) ) {
           af_enqueue();
       }
   } );
   ```

`af_enqueue()` is idempotent — calling it on a request that already enqueued AF's assets is a no-op.

## See also

- [How to display a form in a modal](./how-to-display-a-form-in-a-modal/) — the most common scenario where this comes up.
