---
title: "Heroku Addon"
---

# Heroku Addon Documentation

AppSignal is an [add-on](http://addons.heroku.com) for providing performance monitoring for your Ruby on Rails apps.
To measure is to know. Detailed metrics help to build better, faster Ruby on Rails apps.
Improve performance, debug errors, track deploys and compare hosts all in one complete tool.

![AppSignal error screen](https://s3.amazonaws.com/heroku.devcenter/heroku_assets/images/201-original.jpg 'AppSignal error screen')

* View and inspect errors, recent or from the past
* Error metrics per deploy
* Detailed events break-down for actions
* Error & throughput metrics
* Mean + 90th percentile metrics and graphs
* Leverage ActiveSupport::Notifications
* See who deployed what & when

Our plans are entirely based on the number of web-requests your site receives.
This can be very beneficial for sites that use a big number of dyno's or have a low number of requests on only one dyno.
We __don't__ charge for extra dyno's.
[Take a look at our plans](http://addons.heroku.com/appsignal).


Does your site generate more requests then your plan can handle? Don't worry, we will contact you about getting a plan upgrade, you won't lose any data.

## Supported frameworks

AppSignal currently supports Rails 3 and 4.

## Provisioning the add-on

AppSignal can be attached to a Heroku application via the command line

    :::term
    $ heroku addons:add appsignal
    -----> Adding appsignal to sharp-mountain-4005... done, v18 (free)

Once AppSignal has been added an `APPSIGNAL_PUSH_API_KEY` setting will be available in the app configuration and will contain the AppSignal api-key. AppSignal uses this to push data to it's servers. It can also be used to generate a deploy hook or config file.

Retrieve the api-key using the `heroku config:get` command.

    :::term
    $ heroku config:get APPSIGNAL_PUSH_API_KEY

## Installing the gem

After installing the gem and deploying your application, it will start aggregating and sending data to AppSignal.

Add the gem to the Gemfile

    :::term
    gem 'appsignal'

Then update application dependencies with bundler.

    :::term
    $ bundle install

Deploy your application to Heroku.

That's it! Your site is now being monitored.

## Dashboard

The AppSignal dashboard allows you to view errors and performance issues, browse web actions and deploys, change settings and invite users into your account.

![single sign-on](https://s3.amazonaws.com/heroku.devcenter/heroku_assets/images/203-original.jpg 'Single sign-on')

The dashboard can be accessed via the CLI:

    :::term
    $ heroku addons:open appsignal
    Opening appsignal for sharp-mountain-4005...

or by visiting the [Heroku apps web interface](http://heroku.com/myapps) and selecting AppSignal from the Add-ons menu.

### Invitations

An email has also been send to the owner of the Heroku app.
With this email it's possible to make an additional owner account and log in on [AppSignal](http://www.appsignal.com).
If you already have an AppSignal account, you can use this invitation to add this app to your current AppSignal account.

Dashboard users won't receive any email notifications about errors.
You can invite additional users in the settings of your account within AppSignal, those users will be able to receive email notifications.

## Deploy Hook

AppSignal likes to know when you have deployed your app. We use deploys to measure improvements in your app and as a way to re-set exception notifications.

### Add a deploy hook in Heroku

Use the [Heroku Labs: Dyno Metadata](https://devcenter.heroku.com/articles/dyno-metadata) feature to automatically set the `revision` config option to the `HEROKU_SLUG_COMMIT` system environment variable. This will automatically report new deploys when the Heroku app gets deployed.

### Using multiple deploy hooks

Unfortunately Heroku only supports one http deploy hook per site. Luckily one of our customers created a simple Heroku app to fix this issue: https://github.com/deadlyicon/deploy-hook-forker

## Using our config file

This is not required, but if you want to customize the gem settings, you can add a config file to your project.
Within the config file you can change the slow request threshold or deactivate your site.

### Generate a config file

`YOUR-API-KEY` is your AppSignal api-key, retrieve this using `heroku config:get APPSIGNAL_PUSH_API_KEY`

    :::term
    $ rails generate appsignal production YOUR-API-KEY

If you deploy your application AppSignal will now use the api-key contained in this config file instead of the `APPSIGNAL_PUSH_API_KEY` environment variable.

## Adding integration gems

AppSignal supports various other databases like MongoDB and Redis. Read more [about our integrations](/ruby/integrations/).

## Migrating between plans

NOTICE: Our Heroku integration is still in a test phase, migrating between plans is not possible yet!

[These](http://addons.heroku.com/appsignal) are all our current plans.

Use the `heroku addons:upgrade` command to migrate to a new plan.

    :::term
    $ heroku addons:upgrade appsignal:medium
    -----> Upgrading appsignal:medium to sharp-mountain-4005... done, v18 ($149/mo)
           Your plan has been updated to: appsignal:medium

## Removing the add-on

AppSignal can be removed via the CLI.
This will destroy all associated data and cannot be undone!

    :::term
    $ heroku addons:remove appsignal
    -----> Removing appsignal from sharp-mountain-4005... done, v20 (free)

## Support

For more information about getting the most out of AppSignal read our [docs](/).

Please [email us](mailto:support@appsignal.com) or visit our [campfire room](https://80beans.campfirenow.com/f08a4) if you have any issues or questions. We are happy to help!
