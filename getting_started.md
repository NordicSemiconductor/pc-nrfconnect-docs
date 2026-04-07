---
---

# Getting started

The following sections describe the architecture for developing nRF Connect for Desktop apps.

## Prerequisites

nRF Connect for Desktop is based on [Electron](https://electron.atom.io/),
[React](https://facebook.github.io/react/), and [Redux](http://redux.js.org/).

You need to be familiar with React and Redux to create apps.
At the very least, you must understand concepts like JSX, props, actions, reducers, and
immutable state.

Development is mostly done in TypeScript, with the tooling provided for such purpose.

### Installing development tools

Install the following tools:

- [Node.js](https://nodejs.org) (at least version 20)
- [Git](https://git-scm.com/downloads)
- An editor with good TypeScript support (e.g. VS Code, Atom, WebStorm)

### Installing nRF Connect

If you only want to develop (existing or new) apps, it is sufficient to have nRF
Connect for Desktop installed as a binary. If you do not have it installed
already, follow the
[instructions on how to install nRF Connect for Desktop](https://docs.nordicsemi.com/bundle/nrf-connect-desktop/page/download_cfd.html).

## Architecture of nRF Connect for Desktop

nRF Connect for Desktop is structured around two main blocks: the core and the apps.
There are also some other projects that aid the development of apps for nRF Connect
for Desktop.

### The core

The core resides in the two projects: `pc-nrfconnect-launcher` and `pc-nrfconnect-shared`.

#### pc-nrfconnect-launcher

[`pc-nrfconnect-launcher`](https://github.com/nordicsemi/pc-nrfconnect-launcher)
contains the following parts:

- Launcher from which the apps are installed and launched; the launcher also coordinates access
  to shared resources, for example if multiple apps use the same serial port to access
  the same device.
- [Electron shell](https://electronjs.org) in which the launcher and the
  apps run

#### pc-nrfconnect-shared

[`pc-nrfconnect-shared`](https://github.com/nordicsemi/pc-nrfconnect-shared)
contains the common code for all apps: UI elements and code to give lower-level
access to hardware.

**Note:** Some common libraries are not only provided during development
and then bundled into the apps. The launcher also provides these
libraries during runtime, so that the individual apps do not have to include the
shared code themselves. This has two advantages for the apps:

* The apps can be smaller.
* The apps are usually platform independent, as the only platform specific parts are in the core.

However, this approach means also that the core is platform-dependent and a
platform-specific variant must be downloaded or compiled.

`pc-nrfconnect-shared` also contains code to use
[nRF Util](https://docs.nordicsemi.com/bundle/nrfutil/page/README.html), the command line tool
used by nRF Connect for Desktop to list devices and in some cases to interact with them.
There is code to install the core of nRF Util as well as needed commands in
specific versions in sandbox, so they are independent of other versions
installed in the system.

Besides common code `pc-nrfconnect-shared` also provides common package
dependencies, scripts and configurations for all official applications and the
core. For example, it includes configurations for [esbuild](https://esbuild.github.io),
[ESLint](https://eslint.org) and [Jest](https://jestjs.io) as well as scripts to
run them.

### The apps

Nordic Semiconductors provides several apps to work with the development kits or
dongles from Nordic Semiconductor. They are all installed and run through the
launcher provided by [the core](#the-core).

Besides using the officially distributed apps, you are also able to run your own
apps. This is described in [Everyday app development](app_development.md).

### The rest

The following projects aid the development of apps for nRF Connect for Desktop:

- [`pc-nrfconnect-boilerplate`](https://github.com/nordicsemi/pc-nrfconnect-boilerplate) -
  a minimal app that can be used as a template for new apps.
- [`pc-nrfconnect-docs`](https://github.com/nordicsemi/pc-nrfconnect-docs) -
  the repo that contains the pages you are reading.
- [`nrf-jlink-js`](https://github.com/nordicsemi/nrf-jlink-js) - for checking
  if the user has the version of SEGGER J-Link installed that is currently
  recommended for nRF Connect for Desktop; this library allows for downloading
  the recommended version.

## Next steps

You are now set up to [develop a new app](./create_new_app),
[work on an existing app](./get_an_existing_app_s_sources) or
[work on the core](./core_development) of nRF Connect for Desktop.
