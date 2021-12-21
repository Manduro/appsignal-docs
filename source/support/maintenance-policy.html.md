---
title: "Maintenance policy"
---

## Version support for languages and packages

At AppSignal we have a support policy for all our integration packages. This defines what compatibility we maintain with programming languages and packages we integrate with. We may drop support for some versions of programming languages and packages over time. Consult this page for our policy. This policy may be subject to change in the future.

## Rationale

There are a lot of versions of programming languages and packages out there. We can't realistically support them all. It would take too much time to maintain and would limit our work to specific syntax and features of the languages/packages.

## Programming languages

We support the maintained versions of a programming language we integrate with. Once a release becomes End of Life (EOL), we will drop support in the next major release of an integration. If possible, we do this by updating the language version requirement in our package definitions so that it will fail to install on older versions.

Consult the maintenance policy per language we integrate with below. If a language we support does not have a maintenance policy, we will default to supporting the latest three major releases if possible.

- [Ruby maintenance policy](https://www.ruby-lang.org/en/downloads/branches/)
- [Elixir maintenance policy](https://hexdocs.pm/elixir/compatibility-and-deprecations.html)
    - [Erlang OTP support](https://hexdocs.pm/elixir/1.12/compatibility-and-deprecations.html#compatibility-between-elixir-and-erlang-otp) is based on which version of Elixir supports which OTP version.
- [Node.js maintenance policy](https://nodejs.org/en/about/releases/)
    - We only support the releases under "Maintenance LTS", "Active LTS" and "Current".
- Front-end JavaScript support policy.
    - We aim for compatibility for most major browsers, down to Internet Explorer 9. All browsers older than this can only supported on a "best effort" basis, and full functionality cannot be guaranteed.
    - It also supports different targets:
        - Electron apps
        - Short-lived processes
        - Serverless functions
        - Statically generated apps
        - React Native/Expo apps

## Packages

We support the maintained versions of a package we integrate with. Once a package version becomes End of Life (EOL), we will drop support in the next major release of an integration. If possible, we do this by updating the package version requirement in our package definitions or in some other way.

Not all packages have a maintenance policy, only supporting the latest release of their package. In this case we only support the three latest major versions, or minor versions if a package hardly ever releases a major version. This support is within reason, as long as we can continue supporting it without spending exuberant amounts of time on maintaining compatability.

## Newer integrations

For new integrations (programming languages and packages) this support policy does not apply. AppSignal will first only target the maintained, or latest, version of a language/package. We will not support older versions immediately unless relevant.
