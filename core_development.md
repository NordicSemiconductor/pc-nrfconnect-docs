---
---

# Developing nRF Connect for Desktop core

Developing the core of nRF Connect for Desktop is a bit different than
[developing an app](./app_development).

You might want to work on the core part if you want to change the launcher or
code that is common for the apps.

As detailed in the [architecture summary](./getting_started#the-core), the
core is split into the following projects:

* [`pc-nrfconnect-launcher`](https://github.com/nordicsemi/pc-nrfconnect-launcher) - for the launcher part
* [`pc-nrfconnect-shared`](https://github.com/nordicsemi/pc-nrfconnect-shared) - for the shared code

## Prerequisites

Besides the [general prerequisites](./getting_started#prerequisites), you need some additional prerequisites for developing the core, depending on your operating system.

### Linux

Install the following additional packages for building the project on Ubuntu Linux:

    apt-get install build-essential python2.7 libudev-dev libgconf-2-4

### Windows

Install the additional tools and configurations using Microsoft's
`windows-build-tools` from an elevated PowerShell or cmd.exe (run as
Administrator):

    npm install --global --production windows-build-tools

### macOS

Installing the [general prerequisites](./getting_started#prerequisites) fulfills all prerequisites
for developing the core on macOS.

## Running the launcher from source

Complete the following steps:

1. Clone the source files from
   [https://github.com/nordicsemi/pc-nrfconnect-launcher](https://github.com/nordicsemi/pc-nrfconnect-launcher).

2. Install the dependencies by running the following command:
   ```
   npm ci
   ```

3. Start the continuous compilation by running the following command:
   ```
   npm run watch:build
   ```
   This transpiles and bundles all code into the `dist` directory. The process
   then watches for changes to source code and re-bundles to `dist` on each change.

4. Open a separate terminal window and run the following command:
   ```
   npm run app
   ```
   This opens [Electron](./getting_started.md#prerequisites), which loads its content from `dist`.
   This instance also uses the apps it finds in your `~/.nrfconnect-apps` directory. If you also
   have a binary installation of nRF Connect for Desktop or
   [develop local apps](./app_development), the same apps show up in this
   instance too.


## Developing common code in pc-nrfconnect-shared

Development of [`pc-nrfconnect-shared`](https://github.com/nordicsemi/pc-nrfconnect-shared) is usually only done by the core nRF Connect for Desktop team, but everyone is welcome to propose changes to it.

Generally, it is recommended to stay up-to-date with the version of `pc-nrfconnect-shared` you are using, especially new releases of the launcher might require that all apps are updated to a certain version of it.

The `pc-nrfconnect-shared` repository contains a [Changelog](https://github.com/nordicsemi/pc-nrfconnect-shared/blob/main/Changelog.md) where the “Steps to upgrade when using this package” sections are especially important to read when upgrading to newer versions.
