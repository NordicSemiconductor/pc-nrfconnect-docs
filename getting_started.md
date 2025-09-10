---
---

# Getting started

## Prerequisites

### Recommended knowledge

nRF Connect is based on [Electron](https://electron.atom.io/),
[React](https://facebook.github.io/react/), and [Redux](http://redux.js.org/).
Some familiarity with React and Redux is recommended in order to create apps.
You should at least understand concepts like JSX, props, actions, reducers, and
immutable state.

It would be fine to develop in JavaScript but we do most of our development in
TypeScript these days. So we recommend that and provide the tooling for it.

### Install development tools

You should have a basic setup and little familiarity with these:

- [Node.js](https://nodejs.org) (at least version 20)
- [Git](https://git-scm.com/downloads)
- An editor with good TypeScript support (e.g. VS Code, Atom, WebStorm)

### Install nRF Connect

If you only want to develop (existing or new) apps, it is sufficient to have nRF
Connect for Desktop installed as a binary. If you do not have it installed
already, just follow the
[instructions on how to install nRF Connect for Desktop](https://docs.nordicsemi.com/bundle/nrf-connect-desktop).

## Architecture of nRF Connect for Desktop

Before starting, you should know the basic structure of nRF Connect for Desktop
with its two main blocks, the core and the apps:

### The core

The core resides in the two projects:

[`pc-nrfconnect-launcher`](https://github.com/NordicSemiconductor/pc-nrfconnect-launcher)
contains

- The launcher from which the apps are installed and launched
- The [Electron shell](https://electronjs.org) in which the launcher and the
  apps run
- The launcher also coordinates access to shared resources, e.g. if multiple
  apps use the same serial port to access the same device.

[`pc-nrfconnect-shared`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared)
contains the common code for all apps: UI elements and code to give lower level
access hardware.

A bit unusual: Some common libraries are not only provided during development
and then bundled into the apps. Instead the launcher also provides these
libraries during runtime, so that the individual apps do not have to include the
shared code themselves.

Providing the common libraries through the launcher at runtime has two
advantages for the apps: The apps can be smaller and they are usually platform
independent, as the only platform specific parts are in the core.

Conversely, this means that the core is platform dependent and a
platform-specific variant must be downloaded or compiled.

`pc-nrfconnect-shared` also contains code to use
[nRF Util](https://docs.nordicsemi.com/bundle/nrfutil), the command line tool
used by nRF Connect for list devices and in some cases to interact with them.
There is code to install the core of nRF Util as well as needed commands in
specific versions in sandbox, so they are independent of other versions
installed in the system.

Besides common code `pc-nrfconnect-shared` also provides common package
dependencies, scripts and configurations for all official applications and the
core. E.g. it includes configurations for [esbuild](https://esbuild.github.io),
[ESLint](https://eslint.org) and [Jest](https://jestjs.io) as well as scripts to
run them.

### The apps

Nordic Semiconductors provides several apps to work with the development kits or
dongles from Nordic Semiconductor. They are all installed and run through the
launcher provided by the core.

Besides using the officially distributed apps, you are also able to run your own
apps. This is described later, when we describe how to develop your own apps.

### The rest

There are some more projects that aid the development of apps for nRF Connect
for Desktop:

- [`pc-nrfconnect-boilerplate`](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate)
  is a very minimal app, which is not useful on its own, but may be used as a
  template to start new apps.
- [`pc-nrfconnect-docs`](https://github.com/NordicSemiconductor/pc-nrfconnect-docs)
  contains these pages to describe how to develop nRF Connect for Desktop
- [`nrf-jlink-js`](https://github.com/NordicSemiconductor/nrf-jlink-js) checks
  if the user has the version of SEGGER J-Link installed that we currently
  recommend and can download the recommended version.

## Next steps

You are now set up to [develop a new app](./create_new_app),
[work on an existing app](./get_an_existing_app_s_sources) or
[work on the core](./core_development) of nRF Connect for Desktop.
