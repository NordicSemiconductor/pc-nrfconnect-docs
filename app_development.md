---
---

# How to do app development for nRF Connect for Desktop

You either [created a project for a new app](./create_new_app) or
[got the sources of an existing app](./get_an_existing_app_s_sources). This is
how you do everyday development now:

## Install dependencies

Like on most Node.js projects, you need to install the dependencies once at the
beginning with `npm ci` and only need to repeat that later if the dependencies
change.

## Compiling

During active development we recommend to run `npm run watch`: It compiles and
packs all your code into `dist/bundle.js` as well as continuously checking the
types. It then waits, watches your code and whenever you save source files it
creates a new `dist/bundle.js` (`Ctrl+C` to stop).

If you just want to compile and pack the code only once, you can use
`npm run build:dev` instead. There is also `npm run build:prod` which too
creates a `dist/bundle.js` once, but contrary to `watch` and `build:dev` it is
optimized for production use (e.g. the bundle is minified).

## Running

Start nRF Connect for Desktop. It will find all apps in
`~/.nrfconnect-apps/local` and through the `package.json` in your project pick
up the `dist/bundle.js` that was build before. Because of this, it display the
app it in the list of all apps with a “local” underneath it and you can launch
it from there.

When you edit the source of the app and it is recompiled (probably by keeping
`npm run watch` running), pressing `Ctrl+R` (Windows or Linux) or `Cmd+R`
(macOS) in the running app window will make it reload.

Chrome Developer Tools can be opened by pressing `Ctrl+Alt+I` (Windows/Linux) or
`Cmd+Option+I` (macOS). We recommend that you
[install the DevTools for React and Redux](./core_development#installing-the-electron-dev-tools).

## Testing

You can run the static checks (like linting and type checking) with
`npm run check` and the unit tests with `npm test`.

## Distribute development versions

When you want to give your development version of an app to others, run
`npm pack`. This will create a file like `pc-nrfconnect-boilerplate-0.0.1.tgz`,
which you can send to others, who are supposed to test your app. Then they can
[install that app locally](./local_app_installation).
