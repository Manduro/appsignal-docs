---
title: "Heroku Logplex Errors"
description: "Learn how to set up Heroku Logplex Errors what the errors mean."
---

-> **Tip:** Make sure a working [Heroku Logplex Logdrain](/heroku/setup-logdrain.html) is setup before continuing with this section.

AppSignal will automatically extract Heroku errors from the Logplex data when a Logdrain is setup, no further action is required.

These errors behave like application errors and will produce incidents and samples. These errors will show up under a dedicated "heroku" namespace on AppSignal.

![Screenshot of AppSignal incident list with a Heroku error](/assets/images/screenshots/heroku/heroku-errors-screenshot.png)


Because the errors are generated from Log lines, the information available is limited, we only get the Error name/code, metadata such as request_id, path, method (GET/POST) etc, which will be available in the "Overview" section of the sample. These values are also available to filter samples by.

The error code is determined by Heroku, for a full list of error codes, see the [Heroku errors documentation](https://devcenter.heroku.com/articles/error-codes).

You'll receive notifications for these errors when notifiers are setup. If you do not wish to receive notifications for these errors, it's recommended to setup [Namespace default notification settings](/application/notification-settings.html).
