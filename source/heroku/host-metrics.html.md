---
title: "Heroku host metrics"
description: "Learn how to set up host metrics for Heroku dyno's for AppSignal using Heroku's logplex system."
---

-> **Tip:** Make sure a working [Heroku Logplex Logdrain](/heroku/setup-logdrain.html) is setup before continuing with this section.

Heroku provides a service to retrieve host metrics from a Dyno through their [Logplex system](https://devcenter.heroku.com/articles/logplex).
 AppSignal provides an endpoint to read these metrics from your app's logs. This is the only way to receive accurate host metrics in AppSignal for Heroku apps. The AppSignal [host metrics](/metrics/host.html) feature does not work reliable on Heroku as it doesn't directly expose runtime metrics for LXC containers in which their Dynos run. Using the normal host metrics collection your Heroku apps will report the host metrics for the parent system on which many Heroku apps run rather than metrics limited to your Dynos.

From the Logplex system we're able to receive the following metrics from your apps per dyno:

* Load average
* Memory total limit
* Memory used

To setup AppSignal to receive these metrics, please follow the guide below.

### 1. Update the AppSignal integration.

First update the Ruby gem to version 2.6.0 or newer.
For Elixir, please update to version 1.6.0 or newer of the AppSignal integration.

We switched from using the dyno's UUID to the name (e.g. `web.1`), to make sure we can match the metrics we get from the Logplex drain with the data we receive from the integration, please make sure to run `2.6.x` or better.

### 2. Disable AppSignal host metrics collection

Set the `enable_host_metrics` config option to `false` ([Ruby](/ruby/configuration/options.html#enable_host_metrics) / [Elixir](/elixir/configuration/options.html#enable_host_metrics) / [Node.js](/nodejs/configuration/options.html#option-enablehostmetrics)).

### 3. Enable host runtime metrics on Heroku

The Heroku host [runtime metrics](https://devcenter.heroku.com/articles/log-runtime-metrics) feature will log the host metrics for your dynos at set intervals to your app's log.

To enable this feature on your Heroku app, run the following command on your console.

```
heroku labs:enable log-runtime-metrics
heroku restart
```

After restarting the app you should see the following appear in your logs every now and then.

```
$ heroku logs --tail
source=web.1 dyno=heroku.2808254.d97d0ea7-cf3d-411b-b453-d2943a50b456 sample#load_avg_1m=2.46 sample#load_avg_5m=1.06 sample#load_avg_15m=0.99
source=web.1 dyno=heroku.2808254.d97d0ea7-cf3d-411b-b453-d2943a50b456 sample#memory_total=21.00MB sample#memory_rss=21.22MB sample#memory_cache=0.00MB sample#memory_swap=0.00MB sample#memory_pgpgin=348836pages sample#memory_pgpgout=343403pages
```

Please see the [Heroku Logplex documentation](https://devcenter.heroku.com/articles/log-runtime-metrics) for more information on how to set this up if you run into problems.

After adding the labs feature, host metrics for your app will appear after a few minutes in the ["host metrics" section](https://appsignal.com/redirect-to/app?to=host_metrics).
