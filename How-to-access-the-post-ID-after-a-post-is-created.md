# How to access the post ID after a post is created

When Advanced Forms creates or updates a post, it stores the post ID on the submission object under the `post` key:

```php
$post_id = AF()->submission['post'];
```

The value is available anywhere **after** Advanced Forms' built-in post-editing handler has run.

## Will this collide with a field of the same name?

No. `AF()->submission['post']` is separate from your form fields — if you have a field literally named `post`, `af_get_field( 'post' )` still returns its value as normal.

## Reading the post ID in a custom submission handler

Advanced Forms' built-in post-editing handler runs on `af/form/submission` at priority 10. To read the post ID in your own handler, use a **priority greater than 10**:

```php
add_action( 'af/form/submission', function ( $form, $fields, $args ) {

    if ( isset( AF()->submission['post'] ) ) {
        $post_id = AF()->submission['post'];

        // Use the post ID here…
    }

}, 20, 3 ); // ← note priority 20, after Advanced Forms' built-in handler.
```

## Using the dedicated post-created / post-updated hooks

When you only care about the creation case (or only the update case), the dedicated hooks are cleaner — they're fired as part of the post-editing handler, so priority doesn't matter and you get the post object directly:

```php
add_action( 'af/form/editing/post_created', function ( $post, $form, $args ) {
    $post_id = $post->ID;

    // Run create-only logic here…

}, 10, 3 );
```

```php
add_action( 'af/form/editing/post_updated', function ( $post, $form, $args ) {
    $post_id = $post->ID;

    // Run update-only logic here…

}, 10, 3 );
```
