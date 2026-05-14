# `af/field/calculated/update_value`

JavaScript action that **triggers** a recalculation of all calculated fields in a form. The plugin already subscribes to this action — call it from your own code whenever you need to force calculated fields to refresh outside the default change-driven flow.

```js
// Force a recalculation of every calculated field in the form.
acf.doAction( 'af/field/calculated/update_value' );

// Or scope to a specific calculated field by name or key.
acf.doAction( 'af/field/calculated/update_value/name=FIELD_NAME' );
acf.doAction( 'af/field/calculated/update_value/key=FIELD_KEY' );
```

The plugin recalculates against the current state of the form, so changes the user has made are reflected in the new values. To react to the result, listen for [`af/field/calculated/value_updated`](../af-field-calculated-value_updated/).

## Modifiers

- `af/field/calculated/update_value` — Triggers a recalculation for all calculated fields in the form.
- `af/field/calculated/update_value/name=FIELD_NAME` — Triggers a recalculation scoped to a specific calculated field by name.
- `af/field/calculated/update_value/key=FIELD_KEY` — Triggers a recalculation scoped to a specific calculated field by key.

## See also

- [`af/field/calculated/value_updated`](../af-field-calculated-value_updated/) — Fires after a calculated field's value has been refreshed.
- [Working with calculated fields](../working-with-calculated-fields/)
