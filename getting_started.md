---
---

# Getting started

## Prerequisites

### Recommended knowledge

nRF Connect is based on [Electron](https://electron.atom.io/),
[React](https://facebook.github.io/react/), and [Redux](http://redux.js.org/).
Some familiarity with React and Redux is recommended in order to create apps.
You should at least understand concepts like JSX, props, actions, reducers, and
immutable state. Also, familiarity with
[ECMAScript 2015](https://babeljs.io/learn-es2015/) syntax such as arrow
functions, modules, const/let, and template strings is recommended.

### Install development tools

You should have a basic setup and little familiarity with these:

- [Node.js](https://nodejs.org) (at least version 10)
- [Git](https://git-scm.com/downloads)
- An editor with good JavaScript support (e.g. VS Code, Atom, WebStorm)

### Install nRF Connect

If you only want to develop (existing or new) apps, it is sufficient to have nRF
Connect for Desktop installed as a binary. If you do not have it installed
already, just follow the
[instructions on how to install nRF Connect for Desktop](https://github.com/NordicSemiconductor/pc-nrfconnect-launcher#using-nrf-connect-for-desktop).

Also create the `~/.nrfconnect-apps/local` directory if it does not already
exist:

- Linux/macOS: `mkdir -p "$HOME/.nrfconnect-apps/local"`
- Windows: `md "%USERPROFILE%\.nrfconnect-apps\local"`

## Architecture of nRF Connect for Desktop

Before starting, you should know the basic structure of nRF Connect for Desktop.

It is currently a bit in flux, because we are transitioning to a different API.
The overall concept stays the same, but sometimes there are differences and then
we will explain the difference between the old and the new API. When developing
new apps, you should stick to the [new API](./api_reference), but when working
on existing apps or the core, you might need to be aware of the
[old API](./old_api_reference).

Which brings us right to the two main blocks the core and the apps:

### The core

The core resides in the two projects
[`pc-nrfconnect-launcher`](https://github.com/NordicSemiconductor/pc-nrfconnect-launcher)
and
[`pc-nrfconnect-shared`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared).
It provides multiple things:

- The launcher from which the apps are installed and launched
- The [Electron shell](https://electronjs.org) in which the launcher and the
  apps run
- The common code for all apps: UI elements and code to give lower level access
  hardware

Where the last part, the common code, resides changes with the new architecture:
Now it is found in project `pc-nrfconnect-shared` (since release
[v4.8.0](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/releases/tag/v4.8.0)
of that project) but previously it was also included in
`pc-nrfconnect-launcher`, where now only legacy variants remain for apps that
have not been converted to the new architecture.

A bit unusual: The common code is not only provided during development and then
compiled into the apps. Instead the launcher also provides these libraries
during runtime, so that the individual apps do not have to include the shared
code themselves.

Providing the common libraries through the launcher at runtime has two
advantages for the apps: The apps can be a lot smaller and they are usually
platform independent, as the only platform specific parts are in the core.

Conversely, this means that the core is platform dependent and a
platform-specific variant must be downloaded or compiled.

Besides common code `pc-nrfconnect-shared` also provides common package
dependencies, scripts and configurations for all official applications and the
core. E.g. it includes configurations for [webpack](https://webpack.js.org),
[ESLint](https://eslint.org) and [Jest](https://jestjs.io) as well as scripts to
run them.

### The apps

[Nordic Semiconductors provides several apps](./supported_apps) to work with the
development kits or dongles from Nordic Semiconductor. They are all installed
and run through the launcher provided by the core.

Besides using the officially distributed apps, you are also able to run your own
apps. This is described later, when we describe how to develop your own apps.

### The rest

There are some more projects that aid the development of apps for nRF Connect
for Desktop:

- Several projects are brought in as dependencies by the core which provide
  access to the hardware:
  - [`nrf-device-setup-js`](https://github.com/NordicSemiconductor/nrf-device-setup-js)
    to setup the devices with firmwares appropriate for an app.
  - [`nrf-device-lister-js`](https://github.com/NordicSemiconductor/nrf-device-lister-js)
    to list devices.
  - [`pc-nrfjprog-js`](https://github.com/NordicSemiconductor/pc-nrfjprog-js) to
    access
    [the `nrfjprog` DLL](https://infocenter.nordicsemi.com/topic/ug_nrf5x_cltools/UG/cltools/nrf5x_nrfjprogdll.html).
  - [`pc-ble-driver-js`](https://github.com/NordicSemiconductor/pc-ble-driver-js)
    to access
    [the `pc-ble-driver` library](https://github.com/NordicSemiconductor/pc-ble-driver).
- [`pc-nrfconnect-boilerplate`](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate)
  is a very minimal app, which is not useful on it's own, but may be used as a
  template to start new apps.
- [`pc-nrfconnect-docs`](https://github.com/NordicSemiconductor/pc-nrfconnect-docs)
  contains these pages to describe how to develop nRF Connect for Desktop.

## Next steps

You are now set up to [develop a new app](./create_new_app),
[work on an existing app](./get_an_existing_app_s_sources) or
[work on the core](./core_development) of nRF Connect for Desktop.
