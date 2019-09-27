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

- [Node.js](https://nodejs.org)
- [Git](https://git-scm.com/downloads)
- An editor with good JavaScript support (e.g. VS Code, Atom, WebStorm)

## Architecture of nRF Connect for Desktop

Before starting, you should know the basic structure of nRF Connect for Desktop.
The two main blocks are the core and the apps:

### The core

The core resides in the project
[`pc-nrfconnect-core`](https://github.com/NordicSemiconductor/pc-nrfconnect-core)
and provides multiple things:

- The launcher from which the apps are installed and launched
- The [Electron shell](https://electronjs.org) in which the launcher and the
  apps run
- The common code for all apps: UI elements and code to give lower level access
  hardware

A bit unusual: The common code is not only provided during development, the
libraries are also provided during runtime, so that the individual apps do not
have to provide them themselves.

Providing the common libraries through the core at runtime has two advantages
for the apps: The apps can be a lot smaller and they are usually platform
independent, as the only platform specific parts are in the core.

Conversely, this means that the core is platform dependent and a
platform-specific variant must be downloaded or compiled.

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
    [the `nrfjprog` command line tool](https://infocenter.nordicsemi.com/topic/ug_nrf5x_cltools/UG/cltools/nrf5x_nrfjprogexe.html).
  - [`pc-ble-driver-js`](https://github.com/NordicSemiconductor/pc-ble-driver-js)
    to access
    [the `pc-ble-driver` library](https://github.com/NordicSemiconductor/pc-ble-driver).
- [`pc-nrfconnect-devdep`](https://github.com/NordicSemiconductor/pc-nrfconnect-devdep)
  provides common package dependencies, scripts and configurations for all
  official applications and the core. E.g. it includes configurations for
  [webpack](https://webpack.js.org), [ESLint](https://eslint.org) and [Jest](https://jestjs.io) as well as scripts to run them.
- [`pc-nrfconnect-boilerplate`](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate)
  is a very minimal app, which is not useful on it's own, but may be used as a
  template to start new apps.

## Next steps

Coming up next is the description of what to do, to develop an app for nRF
Connect for Desktop yourself.

## Install nRF Connect

Before you can create apps, you need to have nRF Connect up and running. Follow
the instructions in the
[project README](https://github.com/NordicSemiconductor/pc-nrfconnect-core) to
either install the binaries or build it from source.

## Create an app project

We recommend using tools like webpack, babel, eslint, and jest when creating
apps. Fortunately, you do not need to configure these yourself. We have created
an
[nRF Connect boilerplate app](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate)
that gives you what you need to get started. Follow the instructions in the
boilerplate app README to create an empty app project. If you are already
familiar with our apps, you could develop your own app based on any of our apps.

### Clone repository

Since _nRF Connect_ expects local apps in

- `$HOME/.nrfconnect-apps/local` (Linux/macOS) or
- `%USERPROFILE%/.nrfconnect-apps/local` (Windows) directory,

make sure your repository is cloned or linked there, e.g.

```
git clone https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate.git
```

### Compiling

When _nRF Connect_ have been installed, you are ready to start the compilation.
Run the following command from the command line, standing in the root folder of
the repository:

    npm install

When the procedure has completed successfully you can run the application by
running:

    npm run dev

The built app can be loaded by _nRF Connect_ launcher.

### Testing

Unit testing can be performed by running:

    npm test
