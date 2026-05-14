# Why does the image/file upload field change when a user is logged in or not?

This discrepancy is down to how ACF renders image and file upload fields in response to WordPress' own capability checks. When a user is not logged in or lacks the relevant permissions (typically the `upload_files` capability), ACF falls back to the default/basic HTML upload field and skips enqueuing the scripts and styles that make up the styled WP media uploader version. This is built into ACF's field rendering logic and it aligns with WordPress' core requirements for using the media library.

The basic HTML upload field renders quite differently to the styled version, which is why the two look so different and need their own CSS. If you want the basic uploader to feel more in line with the rest of your form, you may wish to add some custom CSS for it — see [this example on CSS-Tricks](https://css-tricks.com/snippets/css/custom-file-input-styling-webkitblink/) for a good starting point.

If you'd prefer a consistent experience for every user regardless of their login status, you can force the basic HTML uploader by setting the `uploader` display option to `basic` when displaying your form. This disables the media library UI but doesn't require special permissions or a logged-in user. See the [`uploader` display argument](./display-arguments/#uploader) for examples using both the `[advanced_form]` shortcode and the `advanced_forms()` function.

## See also

- [Gallery field not working for some users](./gallery-field-not-working-for-some-users/) — a related capability issue with the gallery field, which depends on the WP media uploader and so requires a logged-in user with `upload_files`.
