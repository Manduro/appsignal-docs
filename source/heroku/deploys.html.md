---
title: "Heroku Deploy Markers"
description: "Learn how to set up deploy markers for Heroku dyno's for AppSignal using Heroku's logplex system."
---

-> **Tip:** Make sure a working [Heroku Logplex Logdrain](/heroku/setup-logdrain.html) is setup before continuing with this section.

A deploy marker indicates a change in the deployed version of an application. This can be used to group together occurrences of errors and performance issues within a certain time frame. From when the version was deployed until a newer version was deployed. Deploy markers are also required to enable backtrace links for an app.

We normally recommend using the [`revision` config option](/application/markers/deploy-markers.html) to set the correct revision for a deploy.

However, when using Heroku with the [Heroku Labs: Dyno Metadata](https://devcenter.heroku.com/articles/dyno-metadata) enabled it will automatically set the `revision` config option to the `HEROKU_SLUG_COMMIT` system environment variable. This will automatically report new deploys when the Heroku app gets deployed.

-> 📖 Also read our guide on [how to set up deploy markers](/guides/deploy-markers.html).

To enable Dyno metadata run:

```
heroku labs:enable runtime-dyno-metadata -a <app name>
```

While replacing `<app name>` with your app name. And that's it! Deploys will now automatically be tracked by AppSignal.
