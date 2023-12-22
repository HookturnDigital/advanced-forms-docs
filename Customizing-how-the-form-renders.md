# Customizing how the form renders

Advanced Forms offers a vast array of options, hooks, and filters for customizing how a form renders.

## Excluding fields from the form

todo

## Controlling the form title and description

todo

## Customizing the submit button text

todo
You can pass custom submit button text to the [`submit_text` display argument](Display-arguments.md#submittext) when
displaying a form.

## Customizing the form wrapper attributes

todo - document `af/form/attributes` filter

## Adding HTML before and after a form

todo - document the `af/form/enqueue` and `af/form/after` filters

## Adding HTML before the form title

todo - document `af/form/before_title` filter. Note this renders before submission errors.

## Adding hidden fields

See [Working with hidden fields](Working-with-hidden-fields.md).

## Adding HTML before and after all form fields

todo - document `af/form/before_field_wrapper` and `af/form/after_field_wrapper` hooks.
todo - document `af/form/before_fields` and `af/form/after_fields` hooks.

## Adding HTML before and after field groups

todo - document `af/field_group/before_field_group` and `af/field_group/after_field_group` hooks.

## Adding HTML before and after fields

todo - document the `af/field/before_field` and `af/field/after_field` hooks.

## Customizing the form success message

todo - document the `af/form/success_message` filter

## Prefilling form fields

See [Prefilling form fields](Prefilling-form-fields.md).

## Modifying the ACF field array before render

todo - document the `af/field/before_render` filter

## Modifying the field wrapper attributes

todo - document the `af/form/field_attributes`

## Controlling the field instruction placement

todo - mention the form arg
todo - document the `af/field/instruction_placement` filter

## Customise the form restricted output

See [Restricting access to a form](Restricting-access-to-a-form.md).

## Related docs

- [Display arguments](Display-arguments.md)
- [Working with hidden fields](Working-with-hidden-fields.md)
- [Restricting access to a form](Restricting-access-to-a-form.md)
- [Prefilling form fields](Prefilling-form-fields.md)