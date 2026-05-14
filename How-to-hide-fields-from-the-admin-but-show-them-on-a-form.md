# How to hide fields from the admin but show them on a form

When a field group is used both on a form and as a post/user edit screen in the admin, you'll often want some fields visible only on the form — terms-and-conditions checkboxes, hidden tracking fields, fields that map to `post_title` / `post_content` which the admin already has built-in editors for.

Advanced Forms adds a field setting called **Hide from admin?** to every ACF field. When enabled, the field renders normally on a form but is hidden in the WordPress admin field-editing UI.

To use it: edit the field group in **ACF → Field Groups**, expand the field you want to hide, and toggle **Hide from admin?** to **Yes**. Save the field group. The field will disappear from any admin location rules the field group is attached to, while continuing to render on any form that the group is mapped to.

## See also

- [How to hide fields based on the current user's role](./hide-fields-by-user-role/) — for the inverse: hiding fields on the form itself based on the visitor's role.
- [How to edit the post content & post title](./edit-post-content-and-title/) — a common case for "hidden from admin but visible on form".
