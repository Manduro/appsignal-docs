---
title: "Adding sample data to a request"
---

Besides tags it's possible to add more metadata to a transaction or span. This allow apps to overwrite the metadata set by AppSignal integration or add more custom data.

!> **Warning**: Do not use sample data to send personal data such as names or email addresses to AppSignal. If you want to identify a person, consider using a user ID, hash or pseudonymized identifier instead. You can use [link templates](/application/link-templates.html) in combination with [tags](/guides/custom-data/tagging-request.html) to link them back to your own system.

## Custom data

Custom data is a sample data "key" that can be used to set more dynamic values than tags allow. This includes nested objects (JavaScript), maps (Elixir) and hashes (Ruby). It is not possible to filter or search on data set on the "custom_data" key, it is only there to provide an additional area in the interface to list more metadata.

This "custom_data" sample data key accepts nested objects. This will result in the following block on a Incident Sample page for both Exception and Performance samples formatted as JSON.

![custom_data](/assets/images/screenshots/sample_data/custom_data.png)

## Ruby

###^ruby `custom_data`

Set custom data on the sample to add additional debugging data about the sample error or performance issue.

```ruby
Appsignal::Transaction.current.set_sample_data(
  "custom_data",
  :i18n => {
    :locale => "en_GB",
    :default_locale => "en_US"
  }
)
```

## Elixir

###^elixir `params`

Store custom parameters with the `params` key on the sample data. This is usually set by integrations provided by AppSignal for libraries such as Plug and Phoenix. By modifying the `params` on the sample data the data set by the AppSignal library integrations will be overwritten.

```elixir
Appsignal.Span.set_sample_data(
  Appsignal.Tracer.root_span,
  "params",
  %{
    i18n: %{
      locale: "en_GB",
      default_locale: "en_US"
    }
  }
)
```

###^elixir `custom_data`

Set custom data on the sample to add additional debugging data about the sample error or performance issue.

```elixir
Appsignal.Span.set_sample_data(
  Appsignal.Tracer.root_span,
  "custom_data",
  %{
    i18n: %{
      locale: "en_GB",
      default_locale: "en_US"
    }
  }
)
```

## Node.js

Sample data with the following key/value pairs can be set **on the root span**:

```js
const span = tracer.currentSpan();
span.setSampleData("key", { string: "value", number: 123 }); // sets additional sample data
```

The first argument to `setSampleData` takes a `key`, that can be one of the following:

###^nodejs `session_data`

Filled with session data by default, but can be overridden with the following call:

```js
span.setSampleData("session_data", {
  _csrf_token: "Z11CWRVG+I2egpmiZzuIx/qbFb/60FZssui5eGA8a3g=",
});
```

This key accepts nested objects that will be rendered as JSON on a Incident Sample page for both Exception and Performance samples.

![session_data](/assets/images/screenshots/sample_data/session_data.png)

###^nodejs `params`

Filled with framework (such as Express) parameters by default, but can be overridden or filled with the following call:

```js
span.setSampleData("params", { string: "value", number: 123 });
```

This key accepts nested objects and will show up as follows on a Incident Sample page for both Exception and Performance samples, formatted as JSON.

![params](/assets/images/screenshots/sample_data/params.png)

###^nodejs `environment`

Environment variables from a request/background job (typically filled by the default `http` integration, but can be further augmented by other integrations), but can be filled/overridden with the following call:

```js
span.setSampleData("environment", { CONTENT_LENGTH: "0" });
```

This call only accepts a one-level key/value object, nested values will be ignored.
This will result the following block on a Incident Sample page for both Exception and Performance samples.

![environment](/assets/images/screenshots/sample_data/environment.png)

###^nodejs `custom_data`

Set custom data on the sample to add additional debugging data about the sample error or performance issue.

```js
span.setSampleData(
  "custom_data",
  {
    i18n: {
      locale: "en_GB",
      default_locale: "en_US"
    }
  }
);
```

## Next steps

- [Filtering app data](/guides/filter-data/) - next guide

---

- [Adding tags to a request](/guides/custom-data/tagging-request.html) - previous step in this guide
- [Tagging and Sample data guide](/guides/custom-data/) - the start of this guide
- [Tagging and Sample data topic](/application/tagging.html) - topic on tagging
- [Getting started guides](/guides/) - Guides overview
