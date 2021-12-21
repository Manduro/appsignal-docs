---
title: "AppSignal for JavaScript configuration"
---

## Available options

The `Appsignal` object can be initialized with the following options.

| Config key | Type | Description  |
| ------ | ------ | ----- |
|  key  |  string  |  Your AppSignal Front-end error monitoring API key.  |
|  uri  |  string  |  (optional) The full URI of an AppSignal Push API endpoint. This setting will not have to be changed. |
|  namespace  |  string  |   (optional) The default namespace to report app errors on AppSignal.com. Read more [about namespaces](/guides/namespaces.html). |
|  revision  |  string  |   (optional) Set the app revision for the currently running version of your app. Used to create deploy markers and tag all incoming data to a certain deploy for the host. Read more about deploy markers on the [deploy markers](/application/markers/deploy-markers.html). |
|  ignoreErrors  |  RegExp[]  |   (optional) An array of `RegExp`s to check against the `message` property of a given `Error`. If it matches, the error is ignored. Do not use a global flag in any regular expressions, as it may cause [unexpected behavior](http://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test#using_test_on_a_regex_with_the_global_flag) leading to some errors not being ignored. Read more about [ignoring errors](/guides/filter-data/ignore-actions.html). |
