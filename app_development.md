---
---

# How to do app development for nRF Connect for Desktop

You either [created a project for a new app](./create_new_app) or
[got the sources of an existing app](./get_an_existing_app_s_sources). This is
how you do everyday development now:

## Install dependencies

Like on most Node.js projects, you need to install the dependencies once at the
beginning with `npm install` and only need to repeat that later if the
dependencies change.

## Compiling

Run `npm run build` to compile and pack everything once, which creates
`dist/bundle.js`.

During active development we recommend to run `npm run dev` instead, which
watches your code and creates a new `dist/bundle.js` repeatedly after every
change (`Ctrl+C` to stop).

## Running

Start nRF Connect for Desktop and it will automatically find the app in
`~/.nrfconnect-apps/local` and display it in the list of all apps with a “local”
underneath it. You can launch it from there.

When you edit the source of the app and it is recompiled (probably by keeping
`npm run dev` running), pressing `Ctrl+R` (Windows or Linux) or `Cmd+R` (macOS)
in the running app window will make it reload.

Chrome Developer Tools can be opened by pressing `Ctrl+Alt+I` (Windows/Linux) or
`Cmd+Option+I` (macOS).

## Testing

You can run the linter with `npm run lint`.

While you can run the tests once with `npm test`, we recommend to keep them
running repeatedly during development with `npm run test-watch`.
