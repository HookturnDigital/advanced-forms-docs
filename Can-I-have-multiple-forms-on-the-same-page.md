# Can I have multiple forms on the same page?

Yes — you can render as many Advanced Forms forms as you like on a single page. Each form is processed independently on submission, so they don't interfere with each other's data or handlers.

There is one known caveat to be aware of: ACF 5.11 introduced a validation routine that scans required fields across the *whole page* rather than scoping to the form being submitted. With more than one form on the page, submitting one form can fail validation on required fields in the other form — fields the user never touched. There's an open issue on the [ACF Pro GitHub repository](https://github.com/AdvancedCustomFields/acf-pro/issues/1000) tracking the upstream fix.

If you hit this, drop in the JS-and-PHP hotfix documented here: [Hotfix for multiple forms on same page triggering validation across all fields](./hotfix-for-multiple-forms-on-same-page-triggering-validation-across-all-fields/). It overrides ACF's validation routine to scope to the submitting form only, and is safe to remove once ACF resolves the issue upstream.

## See also

- [Hotfix for multiple forms on same page triggering validation across all fields](./hotfix-for-multiple-forms-on-same-page-triggering-validation-across-all-fields/)
