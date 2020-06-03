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

### Compilation of native modules

The project depends on
[pc-ble-driver-js](https://github.com/NordicSemiconductor/pc-ble-driver-js) and
[pc-nrfjprog-js](https://github.com/NordicSemiconductor/pc-nrfjprog-js) which
are native modules. Pre-compiled binaries for these modules are provided for
recent Node.js versions on Windows, macOS, and Linux. However, if binaries do
not exist for your platform/Node.js version, then refer to the
[pc-ble-driver-js README](https://github.com/NordicSemiconductor/pc-ble-driver-js)
which describes requirements for compilation.

## Running the launcher from source

Fetch the source from
[https://github.com/NordicSemiconductor/pc-nrfconnect-launcher](https://github.com/NordicSemiconductor/pc-nrfconnect-launcher)
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

## Developing common code in [pc-nrfconnect-shared](https://github.com/NordicSemiconductor/pc-nrfconnect-shared)

When you are developing common code in `pc-nrfconnect-shared` and you want to
check the changes quickly in the launcher or an app the steps above are not
sufficient, because by default the launcher will include a released version of
`pc-nrfconnect-shared` from GitHub, not you local one. But with some additional
effort you can achieve this by leveraging
[`npm-link`](https://docs.npmjs.com/cli/link):

1. Have both, `pc-nrfconnect-launcher` and `pc-nrfconnect-shared` checked out
   into directories next to each other.
2. In the directory `pc-nrfconnect-launcher` run
```
npm install; npm link ../pc-nrfconnect-shared
```
3. In the directory `pc-nrfconnect-shared` run
```
npm ci --prod
```

With this setup, you can make changes in `pc-nrfconnect-shared`, then recompile
`pc-nrfconnect-launcher` (`pc-nrfconnect-shared` does not need to be compiled),
and immediately see the effects of the changes in `pc-nrfconnect-shared`.
Usually the best setup is again to use `npm run dev` in
`pc-nrfconnect-launcher`, as described above in
[“Running the launcher from source”](#running-the-launcher-from-source).

If you forget to run `npm ci --prod` in `pc-nrfconnect-shared`, you may get
errors because of conflicting package versions, especially of `react` and
`react-redux`.

### Caveat:

If you later run `npm install` in the directory `pc-nrfconnect-launcher` (e.g.
because you install additional packages), the link to `pc-nrfconnect-shared`
often gets lost. In that case you usually have to repeat running
`link ../pc-nrfconnect-shared` in the directory `pc-nrfconnect-launcher` and
then `npm ci --prod` in the directory `pc-nrfconnect-shared`.

If you later run `npm install` in the directory `pc-nrfconnect-shared` (e.g.
because you install additional packages there), you may need to repeat running
`npm ci --prod` in that folder.

Because `npm ci --prod` does not install the development dependencies, you
cannot run the tests successfully in `pc-nrfconnect-shared` at that moment.
Before you want to run the tests again, execute `npm ci` and you will also have
the development dependencies installed again.

## Testing

Relevant scripts for different testing needs:

- `npm run lint`: Lint the source
- `npm test`: Run the unit tests once
- `npm run test-watch`: Run unit tests and watch for changes
- `npm run test-e2e`: The launcher additionally included end-to-end tests

## Installing the Electron dev tools

During development the
[React DevTools](https://github.com/facebook/react/tree/master/packages/react-devtools)
and the [Redux DevTools](https://github.com/zalmoxisus/redux-devtools-extension)
are really handy. The easiest way to install them is to run

    npm run install-devtools

when you have the source of the core checked out as described above. You only
need to run this command once, the tools will stay installed in subsequent runs
of Electron.

If you want to remove all dev tools later run

    npm run remove-devtools
