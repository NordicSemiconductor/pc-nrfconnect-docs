---
---

# How to do development of the core of nRF Connect for Desktop

Developing the core of nRF Connect for Desktop is a bit different than
[developing an app](./app_development) for it.

As you have read in the [architecture summary](./getting_started#the-core) the
core is split up over two projects:
[`pc-nrfconnect-launcher`](https://github.com/NordicSemiconductor/pc-nrfconnect-launcher)
and
[`pc-nrfconnect-shared`](https://github.com/NordicSemiconductor/pc-nrfconnect-shared).

These projects are responsible for multiple things. So there could be several
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

## Running the launcher from source

Fetch the source from
[https://github.com/NordicSemiconductor/pc-nrfconnect-launcher](https://github.com/NordicSemiconductor/pc-nrfconnect-launcher)
and like on most Node.js projects, you need to install the dependencies once at
the beginning with `npm install` and only need to repeat that later if the
dependencies change.

Start the continuous compilation by running:

    npm run watch:build

This will transpile and bundle all code into the `dist` directory. The process
will watch for changes to source code, and re-bundle to `dist` on each change.

Now, open a separate terminal window and run:

    npm run app

This will open Electron, which loads its content from `dist`. This instance also
uses the apps it finds in your `~/.nrfconnect-apps` directory. Thus if you also
have a binary installation of nRF Connect for Desktop or
[develop local apps](./app_development), the same apps will show up in this
instance too.

## Developing common code in [pc-nrfconnect-shared](https://github.com/NordicSemiconductor/pc-nrfconnect-shared)

Development of `pc-nrfconnect-shared` is usually only done by the core nRF
Connect for Desktop team, but everyone is welcome to propose changes to it.

It contains a
[Changelog](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/main/Changelog.md)
where the “Steps to upgrade when using this package” sections are especially
important to read when upgrading to newer versions. Generally it is recommended
to stay up-to-date with the version of `pc-nrfconnect-shared` you are using,
esp. new releases of the launcher might require that all apps are updated to a
certain version of it.
