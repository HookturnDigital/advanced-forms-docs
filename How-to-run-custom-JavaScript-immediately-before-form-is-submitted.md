# How to run custom JavaScript immediately before form is submitted

Submission steps let you intercept a form submission and run custom JavaScript right before the request is sent to the server — useful for firing tracking events, validating something client-side, or transforming the payload.

```js
acf.addAction('af/form/setup', function (form) {

    af.addSubmissionStep(form, 10, function (callback) {

        // Run your custom JavaScript here.

        // Call callback() to release the submission chain. If you don't call
        // it, the form won't submit and any later submission steps won't run.
        callback();
    });
});
```

The integer (`10` above) is the priority — lower runs earlier. Use higher priorities to run after other submission steps registered by your code or by third-party integrations.

If your custom JS needs to do something async (e.g. a `fetch` to your own API), invoke `callback()` only after the async work resolves — that way the form waits for your step before continuing.
