# Functions reference

## advanced_form()

```php
advanced_form( $form_key_or_id, $args = [] )
```

Renders a form by a form key or form post ID. For example:

```php
advanced_form( 'form_62bd15508b9c9' );
// or
advanced_form( 123 );
```

For a list of available arguments, see [Display arguments](Display-arguments.md).

## af_get_field()

```php
$value = af_get_field( $field_key_or_name, $args = [] )
```

Gets the value of a desired field from the current form submission. For example:

```php
$value = af_get_field( 'field_62bd15508b9c9' );` or `$value = af_get_field( 'Name' );
```

The optional `$args` array supports the following arguments:

- `fields` - An array of ACF field arrays to search for the desired field. Defaults to all fields in the current
  submission.
- `formatted` - Whether to return the formatted value or the raw value. Defaults to `true`.

## af_save_field()

```php
af_save_field( $field_key_or_name, $object_id )
```

Saves the value of a submitted field directly to an object (post, user, term). The `$object_id` can be any format
supported by ACF's core `acf_decode_post_id()`
function ([See GitHub](https://github.com/search?q=repo%3AAdvancedCustomFields%2Facf+function+acf_decode_post_id&type=code)).
Some common examples include:

- `$post_id` - To save to a post.
- `$taxonomy . '_' . $term_id` - To save to a term in a specific taxonomy.
- `'term_' . $term_id` - To save to a term.
- `'user_' . $user_id` - To save to a user.
- `'option'` - To save to the options table.

## af_save_all_fields()

```php
af_save_all_fields( $object_id, $excluded_fields = [] )
```

Saves all submitted fields directly to an object (post, user, term). The `$object_id` is as
per [af_save_field()](#af_save_field).

## af_register_form()

```php
af_register_form( $form )
```

Registers a form via PHP for use with the `advanced_form()` function. You may use this to create forms without using the
UI. See [Registering forms programmatically](Registering-forms-programmatically.md) for more information.

## af_get_form()

```php
$form = af_get_form( $form_key_or_id )
```

Gets a form array by a form key or form post ID. For example:

```php
$form = af_get_form( 'form_62bd15508b9c9' );` or `$form = af_get_form( 123 );
```

## af_get_forms()

```php
$forms = af_get_forms()
```

Gets an array of all registered forms.