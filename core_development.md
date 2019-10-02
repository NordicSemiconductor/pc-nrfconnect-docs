---
---

# How to do development of the core of nRF Connect for Desktop

Developing
[the core of nRF Connect for Desktop](https://github.com/NordicSemiconductor/pc-nrfconnect-core)
is a bit different than [developing an app](./app_development) for it.

As you have read in the
[architecture summary about the core](./getting_started#the-core), the core
project is responsible for multiple things. So there could be several
motivations for working on it: Most probably you want to change the launcher or
code that is common for the apps.

## Prerequisites

Besides the [general prerequisites](./getting_started#prerequisites) you may
need some additional ones for developing the core:

### Linux

Install additionally required packages for building the project on Ubuntu Linux:

    apt-get install build-essential python2.7 libudev-dev libgconf-2-4

### Windows

Install additionally required tools and configurations using Microsoft's
`windows-build-tools` from an elevated PowerShell or CMD.exe (run as
Administrator):

    npm install --global --production windows-build-tools

### macOS

With macOS you already should have everything installed when you fulfill the
[general prerequisites](./getting_started#prerequisites).

### Compilation of native modules

The project depends on
[pc-ble-driver-js](https://github.com/NordicSemiconductor/pc-ble-driver-js) and
[pc-nrfjprog-js](https://github.com/NordicSemiconductor/pc-nrfjprog-js) which
are native modules. Pre-compiled binaries for these modules are provided for
recent Node.js versions on Windows, macOS, and Linux. However, if binaries do
not exist for your platform/Node.js version, then refer to the
[pc-ble-driver-js README](https://github.com/NordicSemiconductor/pc-ble-driver-js)
which describes requirements for compilation.

## Running from source

Fetch the source from
[https://github.com/NordicSemiconductor/pc-nrfconnect-core](https://github.com/NordicSemiconductor/pc-nrfconnect-core)
and like on most Node.js projects, you need to install the dependencies once at
the beginning with `npm install` and only need to repeat that later if the
dependencies change.

Start the continuous compilation by running:

    npm run dev

This will transpile and bundle all code into the `dist` directory. The process
will watch for changes to source code, and re-bundle to `dist` on each change.

Now, open a separate terminal window and run:

    npm run app

This will open Electron, which loads its content from `dist`. This instance also
uses the apps it finds in your `~/.nrfconnect-apps` directory. Thus if you also
have a binary installation of nRF Connect for Desktop or
[develop local apps](./app_development), the same apps will show up in this
instance too.

### Testing

Relevant scripts for different testing needs:

- `npm run lint`: Lint the source
- `npm test`: Run the unit tests once
- `npm run test-watch`: Run unit tests and watch for changes
- `npm run test-e2e`: Run all end-to-end tests
- `npm run test-e2e-offline`: Run only end-to-end tests that do not require
  network access
- `npm run test-e2e-online`: Run only end-to-end tests that require network
  access
