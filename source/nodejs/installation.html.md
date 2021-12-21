---
title: "Installation"
---

Please follow the [installation guide](/guides/new-application.html) first, when adding a new application to AppSignal.

## Requirements

Before you can compile the AppSignal package, make sure the build/compilation tools are installed for your system. Please check the [Supported Operating Systems](/support/operating-systems.html) page for any system dependencies that may be required.

## Installation

First, [sign up](https://appsignal.com/users/sign_up) for an AppSignal account and run our automated install tool, which will install `@appsignal/nodejs` and any relevant integrations to your project:

```bash
npx @appsignal/cli install
```

You can also skip the automated tool and add `@appsignal/nodejs` to your `package.json` on the command line with `npm`/`yarn`:

```bash
yarn add @appsignal/nodejs
npm install --save @appsignal/nodejs
```

Alternatively, you can manually add the `@appsignal/nodejs` package to your `package.json`. Then, run `yarn install`/`npm install`.

```json
// package.json

{
  "name": "my-app",
  "dependencies": {
    "@appsignal/nodejs": "^1.0.0",
    "@appsignal/express": "^1.0.0"
  }
}
```

You can then import and use the package in your bundle:

```js
const { Appsignal } = require("@appsignal/nodejs");

const appsignal = new Appsignal({
  active: true,
  name: "<YOUR APPLICATION NAME>"
  pushApiKey: "<YOUR API KEY>" // Note: renamed from `apiKey` in version 2.2.5
});

// ...all the rest of your code goes here!
```

!> To auto-instrument modules, the Appsignal module must be both **required** and **initialized** before any other package.

---

-> 📖 Continue with our [installation guide](/guides/new-application.html).

## Uninstalling AppSignal for Node.js

Uninstall AppSignal from your app by following the steps below. When these steps are completed your app will no longer send data to the AppSignal servers.

1. In the `package.json` of your app, delete all lines referencing an `appsignal` package: `"*appsignal/*": "*"`.
2. Run `npm install` or `yarn install` to update your `package.lock`/`yarn.lock` with the removed packages state.
  - Alternatively, run `npm uninstall @appsignal/<package name>` or `yarn remove @appsignal/<package name>` to uninstall an AppSignal package.
3. Remove any AppSignal [configuration](/nodejs/configuration/) from your app.
4. Commit, deploy and restart your app.
  - This will make sure the AppSignal servers won't continue to receive data from your app.
5. Finally, [remove the app](/guides/application/deleting-applications.html) on AppSignal.com.

-> 📖 Continue with our [uninstall guide](/guides/application/deleting-applications.html).
