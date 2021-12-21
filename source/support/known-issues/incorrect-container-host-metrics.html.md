---
title: Incorrect container host metrics reported
---

## Affected components

- AppSignal for Ruby gem versions `1.x` - `2.9.11`.
- AppSignal for Elixir package versions `0.0.x` - `1.10.11`.
- Systems: Containerized systems such as Docker and Heroku (LXC).

## Description

Host metrics reported by AppSignal in the host metrics feature are incorrect, reporting the data for the container's host system instead.

AppSignal uses a library called [probes-rs], created by AppSignal, to report host metrics for the host on which the [AppSignal agent] is running. It does this by reading the `/proc` virtual filesystem. This directory however, always reports data from the parent host rather than the container's constraint resources. This is why the AppSignal agent is reporting the wrong metrics.

## Symptoms

- In the AppSignal host metrics feature the host metrics do not match the container's restraints, reporting resources many times its allowed amount.

## Solution

Please upgrade to the latest AppSignal for Ruby gem or Elixir package to receive accurate host metrics for container metrics.

- Memory host metrics were fixed in Ruby gem `2.8.0` and Elixir package `1.9.0`.
- CPU host metrics were fixed in Ruby gem `2.9.11` and Elixir package `1.10.11`.
- Heroku is supported through our [Logplex log drain for Heroku](/heroku/host-metrics.html).

**Warning**: For container host metrics to be accurate, limits need to be set for every container. This means, configuring your container to have a limited number of CPUs and memory allocated to it. Without these limits the container reports the maximum possible value of these metrics, resulting in the host reporting Terabytes of available memory for example. A container without swap configured, or unsupported on the host system, will report a `0` value. For more information on how to limit your container's CPU and memory, please see the documentation on:

- Docker
  - [CPU](https://docs.docker.com/config/containers/resource_constraints/#cpu)
  - [memory](https://docs.docker.com/config/containers/resource_constraints/#memory)
- Kubernetes
  - [CPU](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/)
  - [memory](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/)

For more information, please read our page on [container metrics and container limits](/metrics/host-metrics/containers.html#container-limits).

Contact us if host metrics are not reported accurately for your system. In which case it would help us a lot if you could send us the `appsignal.log` file and some information about your app's container setup so we can reproduce the issue.

Let us know which metrics you are missing, are inaccurate or if you have a problem, at [support@appsignal.com].

## Workaround

None available at this time.

[probes-rs]: https://github.com/appsignal/probes-rs
[AppSignal agent]: /appsignal/how-appsignal-operates.html#agent
[support@appsignal.com]: mailto:support@appsignal.com
