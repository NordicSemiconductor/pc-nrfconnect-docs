---
---

# Migrating apps

We are currently switching
[the architecture for how apps are written](getting_started#architecture-of-nrf-connect-for-desktop)
from an [old API](old_api_reference) to a [new API](api_reference). We will
continue to support the old API for some time but might eventually phase it out
and apps using the new API are the only ones getting the new look and feel, so
every app should be updated to the new API eventually.

Before migrating an app, you should be familiar with
[the overall concepts of nRF Connect for Desktop](getting_started) and the
[new API](api_reference).

As a reference on what can be done for the migration, have a look at the
[PR 34](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate/pull/34)
and especially the
[commit `677e670`](https://github.com/NordicSemiconductor/pc-nrfconnect-boilerplate/pull/34/commits/677e6707dd112292027fe2655ff18f63e56447dc)
of the project `pc-nrfconnect-boilerplate`. This gives you a concrete example on
some of the essential steps that need to be done as described below.

## Essentials steps to upgrade an app

1. Specify that your app requires as engine at least the version 3.4 of
   `nrfconnect`, as this version of the core started providing the new API.

2. Upgrade the dependency to `pc-nrfconnect-shared` (or `pc-nrfconnect-devdep`,
   as it was called before) to at least version 4.8, as this is the one that
   introduced the baseline for the new design.

   The upgrade of this dependency may need some additional minor things along
   the way:

   - Because `react`, `react-redux` and `react-dom` were turned into peer
     dependencies, they have to be added as dev dependencies in most projects
     (react-dom is not always needed, but it often is easier to just add it
     instead of figuring out whether it is needed):

     ```bash
     npm install --save-dev --save-exact react react-redux react-dom
     ```

   - The linting rules were slightly strengthened so often some new errors pop
     up there. Many can be auto fixed, so run `npm run lint -- --fix` first and
     then review the changes made as well as the remaining issues.

   - Also run the tests and check whether something needs to be corrected (e.g.
     because `jest` was updated). E.g. in the BLE app, references to
     `require.requireActual` needed to be corrected to `jest.requireActual`.

3. As described in the [new API](api_reference) the entry point of your package
   now must expose a React component, while it previously exposed a bunch of
   config objects and functions that had to be named according to the
   [old API](old_api_reference).

   For your entry point you will now usually use
   [the new `App` component](api_reference#component-app) and pass according
   props to it. These props will take over what was previously found in the old
   config objects and functions. These are the typical migrations for this:

   - `decorateNavMenu` and `decorateMainView` → The menu items and main views
     become the `panes` property of the `App` component.
   - `decorateSidePanel` → The property `sidePanel` of the `App` component.
   - `reduceApp` → The property `appReducer` of the `App` component.
   - Old `config` object → Properties to a
     [`DeviceSelector` component](api_reference#component-deviceselector).
   - `middleware` → This configuration was mostly used to react on certain
     lifecycle moments of selecting a device and this is now done using the
     callbacks of the
     [`DeviceSelector` component](api_reference#component-deviceselector).
   - `map*Dispatch` and `map*State` -> In the new architecture you write
     everything below the `App` component as normal React components and if you
     want to interact with theRedux state then use React Redux either with
     [hooks](https://react-redux.js.org/api/hooks) or
     [the `connect` function](https://react-redux.js.org/api/connect).

4. Search for all remaining places where `nrfconnect/core` is imported and
   replace it with according imports of `nrfconnect-shared`. This is often just
   a simple replacement, e.g. [the logger](api_reference#logging-logger) is now
   exported from there.

5. If your app accessed the state of the core before: It is not supported
   anymore to depend on a certain shape of the state. Instead use selectors (if
   there is not an appropriate selector, then ask for one and we will introduce
   it in `nrfconnect-shared`).

## Recommended, additional things to do when upgrading an app

These steps are not strictly necessary but strongly recommended cleanups:

1. If there still is a `.babelrc` in your project, then remove it.
   `pc-nrfconnect-shared` now provides
   [a common babel configuration](https://github.com/NordicSemiconductor/pc-nrfconnect-shared/blob/master/config/babel.config.js)
   used for all projects.

2. Add a file `.huskyrc.json` to your project with the following content, so
   that tests and linting is run automatically before every push:

   ```json
   {
     "hooks": {
       "pre-push": "bash node_modules/pc-nrfconnect-shared/scripts/pre-push.sh"
     }
   }
   ```

3. Rename the directory `lib` to `src` and move the entry file `index.jsx` into
   the directory `src`.

4. [Stop grouping files by technology and instead group them by domain](https://gist.io/@datenreisender/64cf8d86069f40108789b6752412513b).
   For example break up directories like `components` or `reducers` and instead
   e.g. put everything belonging to the side panel into a directory on it's own.

   This includes moving tests to what they test and (S)CSS files to what they
   style.

5. Stop using `immutable-js`
   [as it seems essentially unmaintained](https://github.com/immutable-js/immutable-js/issues/1689)
   and
   [the Redux style guide also rather recommends using `immer`](https://redux.js.org/style-guide/style-guide#use-immer-for-writing-immutable-updates)
   nowadays.
