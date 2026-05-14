# `af/field/calculated/value_updated`

JavaScript action that **fires after** a calculated field's value has been refreshed. Use it to react to the new value — for example, to mirror the value into another part of the page, run a downstream calculation, or update an external preview.

```js
acf.addAction( 'af/field/calculated/value_updated', function( value, $field, form ) {
    // Mirror the calculated value into a preview element on the page.
    $( '#preview-container' ).html( value );
});
```

The handler receives the recalculated `value`, the `$field` jQuery element, and the `form` object. The action fires once per calculated field on every refresh.

## Modifiers

- `af/field/calculated/value_updated` — Fires for every calculated field on every refresh.
- `af/field/calculated/value_updated/name=FIELD_NAME` — Fires only for the calculated field with a specific name.
- `af/field/calculated/value_updated/key=FIELD_KEY` — Fires only for the calculated field with a specific key.

## See also

- [`af/field/calculated/update_value`](../af-field-calculated-update_value/) — Trigger a recalculation manually.
- [Working with calculated fields](../working-with-calculated-fields/)
