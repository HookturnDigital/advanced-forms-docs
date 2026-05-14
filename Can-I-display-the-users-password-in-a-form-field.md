# Can I display the user's password in a form field?

No — WordPress doesn't store passwords in plain text. They're stored as a one-way hash that can't be reversed, which is what stops them being leaked if someone gains access to the database. There's no value to read back into a field.

If your goal is to let a user change their password from a form, take that path instead: add a password field, hook into the form submission, and call [`wp_set_password()`](https://developer.wordpress.org/reference/functions/wp_set_password/) with the submitted value. See [_How to update a user's password from a form field_](./how-to-update-a-users-password-from-a-form-field/) for the full pattern.
