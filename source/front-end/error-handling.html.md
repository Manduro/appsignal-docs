---
title: "Frontend error catching"
---

To report an error to AppSignal, catch the error and pass it to the `sendError` helper function.

```javascript
try {
  // do something that might throw an error
} catch (error) {
  appsignal.sendError(error)
  // handle the error
}

// You can catch errors asynchronously by listening to Promises...
asyncActionThatReturnsAPromise().catch(error => appsignal.sendError(error))

// ...or by using async/await
async function() {
  try {
    const result = await asyncActionThatReturnsAPromise()
  } catch (error) {
    appsignal.sendError(error)
    // handle the error
  }
}

// ...or in an event handler or callback function
events.on("event", (err) => { appsignal.sendError(err) })
```

## Adding metadata to errors

To add additional metadata to errors sent with `sendError`, use the callback function argument. This callback receives the current Span for the error as an argument and allows it to be modified before being sent to the AppSignal servers.

See the [Span API page][span api] for more information about all the data that can be set on a Span.

```javascript
try {
  // do something that might throw an error
} catch (error) {
  appsignal.sendError(error, (span) => {
    span.setAction("MyCustomAction")
    span.setNamespace("custom_namespace")
    span.setTags({ tag1: "value 1", tag2: "value 2" })
  })
  // handle the error
}
```

## Uncaught exceptions

Uncaught exceptions are **not** captured by default. We made the decision to not include this functionality as part of the core library due to the high amount of noise from browser extensions, ad blockers etc. that generally makes libraries such as this less effective.

We recommend using a relevant [integration](/front-end/integrations/) as a better way to handle exceptions, or, if you *would* prefer capture uncaught exceptions, you can do so by using the [`@appsignal/plugin-window-events`](/front-end/plugins/plugin-window-events.html) package alongside this one.

## Wrapping a block of code

The library provides a convenience method for wrapping a block of code and sending any error thrown within it directly to AppSignal. This is the `appsignal.wrap()` method, an async function that returns a `Promise`. A function should be passed as an argument containing the code you'd like to wrap.

```javascript
try {
  await appsignal.wrap(() => {
    // catch any error from sync or async code here
  })
} catch (e) {
  // do something else with the error here, the
  // error has already been sent to AppSignal
}

// Alternative usage
appsignal.wrap(() => {
  // catch any error from sync or async code here
})
.catch((e) => {
  // do something else with the error here, the
  // error has already been sent to AppSignal
})
```

If an error is thrown anywhere in this function, we return a rejected promise with the `Error` passed as an argument to the `catch` handler.

### Adding metadata to wrapped errors

To add additional metadata to errors captured and sent with `wrap`, use the callback function argument. This callback receives the current Span for the error as an argument and allows it to be modified before being sent to the AppSignal servers.

See the [Span API page][span api] for more information about all the data that can be set on a Span.

```javascript
appsignal.wrap(() => {
  throw new Error("Test error")
}, (span) => {
  span.setAction("MyCustomAction")
  span.setNamespace("custom_namespace")
  span.setTags({ tag1: "value 1", tag2: "value 2" })
})
```

[span api]: /front-end/span.html
