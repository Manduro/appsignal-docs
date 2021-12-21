---
title: "Capistrano"
---

[Capistrano](http://capistranorb.com/) versions 2 and 3 are officially
supported by the AppSignal Ruby gem, but might require some manual
configuration.

The Capistrano integration makes sure an AppSignal deploy marker is created on
every deploy. Read more about
[deploy markers](/appsignal/terminology.html#markers).

## Installation

Make sure you load the `appsignal/capistrano` file in Capistrano's `Capfile`.
This should be done automatically when you run the `appsignal install` command
during installation.

```ruby
# Capfile
require 'capistrano'
# Other Capistrano requires.
require 'appsignal/capistrano'
```

## Configuration

### appsignal_config

```ruby
# deploy.rb
set :appsignal_config, name: 'My app'
```

`appsignal_config` allows you to override any config loaded from the
`appsignal.yml` configuration file.

### appsignal_env (since gem version 1.3)

```ruby
# deploy.rb
set :stage, :alpha
set :appsignal_env, :staging
```

`appsignal_env` allows you to load a different AppSignal environment when a
stage name doesn't match the AppSignal environment as named in the AppSignal
config file or environment variable.

### appsignal_revision (since gem version 0.8.8)

!> **Warning**: This config option is deprecated. Please use the [`revision` config option](/ruby/configuration/options.html#option-revision) instead. For more information, please read the [deploy markers topic](/application/markers/deploy-markers.html).

In Capistrano 2 AppSignal is able to fetch the revision from the Capistrano
config.

In Capistrano 3 however, this is no longer available and setting the
revision is recommended.

```ruby
# deploy.rb
set :appsignal_revision, "my_revision"
```

The revision can be set manually or fetched from the git repository locally.

```ruby
# Sets the current branch's git commit SHA as the revision
set :appsignal_revision, `git log --pretty=format:'%h' -n 1`
```

If you're using the branch configuration setting Capistrano you can also set
git to fetch the commit SHA from the selected branch.

```ruby
set :branch, "main"
# Sets the selected branch's git commit SHA as the revision
set :appsignal_revision, `git log --pretty=format:'%h' -n 1 #{fetch(:branch)}`
```

### appsignal_user (since gem version 2.4.0)

Used in combination with `appsignal_revision` (if necessary), it's possible to customize a deploy user for Capistrano deploys.

By default this uses the system's local username, read from the `USER` or `USERNAME` environment variable, so there's no need to configure it if the system has these variables available.

If you do want to customize the deploy user, set the `:appsignal_user` config option in your Capistrano config.

```ruby
# deploy.rb
set :appsignal_user, "Jane" # Custom deploy user name
```

## Example applications

We have two example applications in our examples repository on GitHub. The
examples show how to set up AppSignal in small Capistrano applications while
loading configuration values from the environment using gems like dotenv and
Figaro.

- [AppSignal + Capistrano + dotenv][example-dotenv-app]
- [AppSignal + Capistrano + Figaro][example-figaro-app]

[example-dotenv-app]: https://github.com/appsignal/appsignal-examples/tree/capistrano+dotenv
[example-figaro-app]: https://github.com/appsignal/appsignal-examples/tree/capistrano+figaro
