---
title: "Exception handling"
---

In most applications some errors will get raised that aren't related to possible bugs in your code, they just happen when your app gets into contact with the real world. Bots might drop by and try to automatically post forms, outdated links might direct visitors to content that doesn't exist anymore and so on.

To avoid these errors from being raised as problems in AppSignal it's possible to add exception handling to your code or even let AppSignal completely ignore certain errors.

## Ignore errors

The AppSignal configuration makes it possible to [ignore errors](/nodejs/configuration/ignore-errors.html). By providing a list of specific errors AppSignal will not send alerts when these errors are raised.

## `tracer.setError(error)`

If you want to catch exceptions in your application to prevent crashes, but still want to track the occurrence you can use `tracer.setError()` to add the exception to the context of the current `RootSpan`:

```js
try {
  throw new Error("Oh no!");
} catch (e) {
  tracer.setError(e);
}
```

The exception will be tracked by AppSignal like any other error, and it allows you to provide custom error handling and fallbacks.

## `tracer.sendError(error, fn(span))`

AppSignal provides a mechanism to track errors that occur in code that's not in a web context. This is useful for instrumentation that doesn't automatically
create AppSignal transactions to profile.

You can use `tracer.sendError()` to directly send an exception to AppSignal from any place in your code without the presence of a `RootSpan` in the given context.

The second argument is a callback function that provides a RootSpan that is created in place so you can add metadata to the tracked error.

```js
try {
  throw new Error("Error!");
} catch (err) {
  tracer.sendError(err, span => {
    span.setName("daily.task"); // Set a recognizable action name
    span.set("user_id", user_id); // Set custom tags
  });
}
```
