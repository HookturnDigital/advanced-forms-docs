# Can I generate a PDF from a form submission?

PDF generation isn't a built-in feature of Advanced Forms, but it's straightforward to wire up with a bit of custom code. The standard pattern is to hook into the form submission, generate a PDF from the submitted field values, save it to the server, and attach it to the notification email.

The basic process:

1. Generate a PDF via PHP, pulling field values with [`af_get_field()`](https://advancedforms.github.io/functions/#af_get_field).
2. Save the PDF to a file on the server (somewhere under `wp-content/uploads/` works well).
3. Return the absolute file path from the [`af/form/email/attachments`](https://advancedforms.github.io/filters/af/form/email/attachments/) filter to attach it to the notification email.

For the attachment step, see [_How to add attachments to a notification email_](./how-to-add-attachments-to-a-notification-email/) — the pattern is the same as attaching any other file, the only difference is that you're generating the file on the fly during submission rather than pointing at a pre-existing one.

## Picking a PDF library

Advanced Forms doesn't ship a PDF generator, so you'll need to bring your own. Popular PHP options include:

- [**Dompdf**](https://github.com/dompdf/dompdf) — renders HTML/CSS to PDF; convenient if you're comfortable templating with HTML.
- [**mPDF**](https://github.com/mpdf/mpdf) — also HTML/CSS-driven, with strong Unicode and right-to-left support.
- [**TCPDF**](https://tcpdf.org/) — long-established, lower-level API for building PDFs programmatically.

Any of them will get the job done; pick the one whose API and output style suits your project. Install via Composer in your theme or a small custom plugin, then call it inside the attachments filter (or earlier, in `af/form/submission`, if you want the PDF available beyond just the email).
