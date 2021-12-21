---
title: "Host metrics"
---

The AppSignal agent collects various system metrics, which allows you to correlate performance issues and errors to abnormal host metrics. This data is available in the [Host metrics](https://appsignal.com/redirect-to/app?to=host_metrics) section in the app overview, which allows you to inspect and [compare multiple hosts](https://blog.appsignal.com/2018/02/20/comparing-hosts.html). We also show a host metrics overview on the sample detail page for error and performance incidents.

For a preview of how host metrics look in the AppSignal interface, please see our [host metrics](https://www.appsignal.com/tour/hosts) tour page.

-> **Note**: This feature is available in these packages:
-> <ul>
-> <li>Ruby: gem version `1.2` and newer. Enabled by default since version `1.3`.</li>
-> <li>Elixir: package version `0.10.0` and newer. Enabled by default since version `0.10.0`.</li>
-> <li>Node.js: all package versions.</li>
-> <li>JavaScript for Front-end: none package versions.</li>
-> </ul>

-> **Note**: This feature is not available on the following architectures:
-> <ul>
-> <li>macOS/OSX (<code>darwin</code>)</li>
-> <li>FreeBSD</li>
-> </ul>
-> A list of supported Operating Systems is available on the [Supported Operating Systems](/support/operating-systems.html) page.

## Collected host metrics

The following host metrics are collected by the AppSignal agent for every minute on your system.

<table>
  <thead>
    <tr>
      <th>Metric</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>CPU usage</td>
      <td>
        User, nice, system, idle and iowait in percentages.
        <br>
        Read more about <a href="https://blog.appsignal.com/2018/03/06/understanding-cpu-statistics.html">CPU metrics</a> in our academy article.
      </td>
    </tr>
    <tr>
      <td>Load average</td>
      <td>1 minute load average on the host.</td>
    </tr>
    <tr>
      <td>Memory usage</td>
      <td>Available, free and used memory. Also includes swap total and swap used.</td>
    </tr>
    <tr>
      <td>Disk usage</td>
      <td>Percentage of every disk used.</td>
    </tr>
    <tr>
      <td>Disk IO</td>
      <td>Throughput of data read from and written to every disk.</td>
    </tr>
    <tr>
      <td>Network traffic</td>
      <td>Throughput of data received and transmitted through every network interface.</td>
    </tr>
  </tbody>
</table>

These host metrics are collected by default. To disable it, use the `enable_host_metrics` configuration option, for [Ruby](/ruby/configuration/options.html#option-enable_host_metrics) and [Elixir](/elixir/configuration/options.html#option-enable_host_metrics).

### Environment metadata

The AppSignal agent reports the following information of the host. Once received this metadata is shown on the [host metrics index page](https://appsignal.com/redirect-to/app?to=host_metrics) per host to provide more detail of all hosts running an app. Use this metadata to spot any differences between hosts that could cause differences in behavior.

- Host architecture, 32 or 64-bit.
- Operating System, either Linux, macOS or Windows.
- Operating System Distribution, for Linux this reports Ubuntu, Fedora, etc.
- Operating System version, the version of the Operating System.
- Kernel version, for Linux this reports the installed kernel version.

This metadata is collected by default. To disable it, use the `send_environment_metadata` configuration option, for [Ruby](/ruby/configuration/options.html#option-send_environment_metadata).

## Heroku support

To use host metrics on Heroku, head to the [Heroku host metrics][heroku support] page.

##=container-support Docker/container support

To use host metrics on (Docker) containers, head to the [container host metrics](/metrics/host-metrics/containers.html) page.

## Dokku support

-> **Note**: Dokku support is available in packages Ruby gem 3.0.13, Elixir package 2.2.0, Node.js package 2.0.0 and newer.

[Dokku](https://github.com/dokku/dokku) has host metrics enabled by default as long as your Dokku application has the `DOKKU_ROOT` environment variable set up.


[heroku support]: /heroku/host-metrics.html
[container support]: /metrics/host-metrics/containers.html
