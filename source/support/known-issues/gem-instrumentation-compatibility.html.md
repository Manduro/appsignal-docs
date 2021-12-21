---
title: Compatibility issue with other instrumentation gems
---

## Affected components

- AppSignal for Ruby versions: `0.x` - `2.11.x`
  - Used in combination with:
    - http_logger gem 0.6.0 and later
    - Bugsnag gem 6.12.0 and later
    - Datadog gem

## Description

Upon executing a rake task or performing a `Net::HTTP` request the app will raise a `stack level too deep (SystemStackError)` error. There may also be other scenarios where this `SystemStackError` occurs, as it can potentially occur with every type of instrumentation set up by the AppSignal gem and other gems.

This error is caused by the AppSignal Ruby gem and other gems with instrumentation having different methods of instrumentation that are incompatible. These different methods cause aliased methods, set up by instrumentation gems, to be continuously called until Ruby's stack has been exhausted.

We've have discussed a solution for this issue in [appsignal-ruby issue #603](https://github.com/appsignal/appsignal-ruby/issues/603). This issue also contains more information about the cause of the issue and potential solutions. Please report issues with other gems if encountered in this issue.

## Solution

Upgrade to the AppSignal for Ruby gem version `3.0.0` or newer.

See our [upgrade guide to Ruby gem 3.0](/ruby/installation/upgrade-from-2-to-3.html) for more information about this issue and upgrading the AppSignal Ruby gem.

## Workaround

- Downgrade the Bugsnag library to `6.11.1`.
- Downgrade the http_logger library to `0.5.1`.
- Downgrade other gems that introduce the same problem in newer versions.
- Remove either of the gems until the issue has been resolved in both gems.
