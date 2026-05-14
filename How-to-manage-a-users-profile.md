# How to manage a user's profile

To let users update their own profile with an Advanced Forms form:

1. Build a form with fields that match the user profile data you want exposed.
2. Drop the form onto an account page with the `user` arg set to `"current"`:

```
[advanced_form form="YOUR_FORM_KEY_HERE" user="current"]
```

On submit, Advanced Forms updates the currently logged-in user with the submitted values.

The same arg works on the function:

```php
advanced_form( 'YOUR_FORM_KEY_HERE', [ 'user' => 'current' ] );
```

## See also

- [How to update a user's password from a form field](./update-user-password-from-a-form-field/)
- [How to conditionally modify the user role on user creation](./change-user-role-user-creation/)
